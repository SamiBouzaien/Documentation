# automapper profile configuration

## First solution

```csharp
public static class Mapping
{
    private static readonly Lazy<IMapper> Lazy = new Lazy<IMapper>(() =>
    {
        var config = new MapperConfiguration(cfg => {
            // This line ensures that internal properties are also mapped over.
            cfg.ShouldMapProperty = p => p.GetMethod.IsPublic || p.GetMethod.IsAssembly;
            cfg.AddProfile<MappingProfile>();
        });
        var mapper = config.CreateMapper();
        return mapper;
    });

    public static IMapper Mapper => Lazy.Value;
}

public class MappingProfile : Profile
{
    public MappingProfile()
    {
        CreateMap<Source, Destination>();
        // Additional mappings here...
    }
}



var destination = Mapping.Mapper.Map<Destination>(yourSourceInstance);

```

## Second solution

```csharp
public class AutoMapperConfiguration
{
    public static void Configure()
    {
        Mapper.Initialize(x =>
            {
                x.AddProfile<MyMappings>();
            });
    }
}

 public class MyMappings : Profile
{
    public override string ProfileName
    {
        get { return "MyMappings"; }
    }

    protected override void Configure()
    {
    ......
    }




void Application_Start()
    {
        AutoMapperConfiguration.Configure();
    }

```
