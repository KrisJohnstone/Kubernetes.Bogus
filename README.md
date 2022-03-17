# Introduction
This library acts as extension for the epic package Bogus by bchavez by providing extension methods in the form
of being able to generate names for various items. It also has the ability to generate a couple of different objects for use in automated tests.

At present its pretty basic and is missing alot of objects. Adding to it as needed for personal projects, if you wish to add or extend existing methods feel free to submit a PR :)

# Installing
Using package manager:
```
Install-Package Kubernetes.Bogus -Version 1.0.2-beta
```

Using the CLI:
```
dotnet add package Kubernetes.Bogus --version 1.0.2-beta
```

# Using/Examples

Creating your own object using kubernetes.bogus:
```
    public static List<SomeModel> MockSomeModel(string ns, int numberOfSomeModel = 1, int numberOfContainers = 1)
    {
        return new Faker<SomeModel>()
            .RuleFor(i => i.Namespace, (faker, _) => ns)
            .RuleFor(i => i.Containers, () => new Faker<Container>()
                .RuleFor(i => i.ContainerName, (faker, _) =>
                    faker.Kubernetes().Container())
                .RuleFor(i => i.ContainerVersion, (faker, _) => faker.Kubernetes().Version())
                .RuleFor(i => i.FullPath, (_, container) => $"{container.ContainerName}:{container.ContainerVersion}")
                .Generate(numberOfContainers)).Generate(numberOfSomeModel);
    }
```

Using object generation to get a list of Deployments:
```
    public static List<V1Deployment> MockGetListOfDeployments(int numberOfPods)
    {
        var depList = new List<V1Deployment>();

        depList.AddRange(KubernetesMethods.GenerateDeployment(numberOfPods));

        return depList;
    }
```
