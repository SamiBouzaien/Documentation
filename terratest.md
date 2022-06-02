# Documentation de terratest

## Configuration de l'environement locale

### Pré-Requis

Sur votre machine, disposez

- [GO](https://https://go.dev/)
- [Terraform CLI](https://www.terraform.io/downloads)
- [Azure CLI sur Windows](https://docs.microsoft.com/fr-ca/cli/azure/install-azure-cli-windows?tabs=azure-cli)

### Configuration

1. Créez un dossier test

2. Pour configurer les dépendances, exécutez :

```bash
cd test
go mod init "[NOM_MODULE]"
```

Où [NOM_MODULE] est le nom de votre module

3. Pour exécuter les tests :

```bash
cd test
go test -v -timeout 30m
```

## Exemples de tests

### Premier exemple

```go
package test

import (
	"testing"
    "fmt"
	"github.com/gruntwork-io/terratest/modules/terraform"
	"github.com/stretchr/testify/assert"
)

func TestTerraformHelloWorldExample(t *testing.T) {

    t.Parallel()
    setTerraformVariables()

    outputName := "hello_world"
    expectedMessage := "Hello, World!"

	// Construct the terraform options with default retryable errors to handle the most common
	// retryable errors in terraform testing.
	terraformOptions := terraform.WithDefaultRetryableErrors(t, &terraform.Options{
		// Set the path to the Terraform code that will be tested.
		TerraformDir: "../examples/terraform-hello-world-example",
	})

	// Clean up resources with "terraform destroy" at the end of the test.
	defer terraform.Destroy(t, terraformOptions)

	// Run "terraform init" and "terraform apply". Fail the test if there are any errors.
	terraform.InitAndApply(t, terraformOptions)

	// Run `terraform output` to get the values of output variables and check they have the expected values.
    t.Run("helloworld_output_matching", func(t *testing.T) {
        output := terraform.Output(t, terraformOptions, outputName)
        fmt.Printf("Output value :: %s\n", output)
        assert.Equal(t, expectedMessage, output)
    }
}
```

### Deuxieme exemple

```go
package test

import (
	"fmt"
	"testing"
	"time"

	http_helper "github.com/gruntwork-io/terratest/modules/http-helper"

	"github.com/gruntwork-io/terratest/modules/terraform"
)

func TestTerraformAwsHelloWorldExample(t *testing.T) {
	t.Parallel()

	setTerraformVariables()

	// Construct the terraform options with default retryable errors to handle the most common
	// retryable errors in terraform testing.
	terraformOptions := terraform.WithDefaultRetryableErrors(t, &terraform.Options{
		// The path to where our Terraform code is located
		TerraformDir: "../examples/terraform-hello-world-example",
	})

	// At the end of the test, run `terraform destroy` to clean up any resources that were created.
	defer terraform.Destroy(t, terraformOptions)

	// Run `terraform init` and `terraform apply`. Fail the test if there are any errors.
	terraform.InitAndApply(t, terraformOptions)

	// Run `terraform output` to get the IP of the instance
	publicIp := terraform.Output(t, terraformOptions, "public_ip")

	// Make an HTTP request to the instance and make sure we get back a 200 OK with the body "Hello, World!"
	url := fmt.Sprintf("http://%s:8080", publicIp)
	http_helper.HttpGetWithRetry(t, url, nil, 200, "Hello, World!", 5, 5*time.Second)
}
```

## Configuration de pipeline IaC

```yaml
trigger:
  - master

variables:
  - group: terraform-app-registration

pool:
  vmImage: "ubuntu-latest"

steps:
  - task: GoTool@0
    displayName: "Install Go tooling"
    inputs:
      version: "1.13.5"

  - task: TerraformInstaller@0
    displayName: "Install Terraform tooling"
    inputs:
      terraformVersion: "0.12.3"

  - bash: |
      cd test
      go mod init workaround
      go test -v
    displayName: "Run the tests"
```
