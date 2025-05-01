---
title: Hello World - Console
---

In this quickstart, we will take a look at a minimum console application that executes a workflow.

We will:

* Programmatically define a workflow definition that displays the text "Hello World" to the console using Elsa's Workflow Builder API.
* Run the workflow.

## The Project

Create a new .NET Core Console project called `ElsaQuickstarts.ConsoleApp.HelloWorld`:

```bash
dotnet new console -n "ElsaQuickstarts.ConsoleApp.HelloWorld"
```

CD into the created project folder:

```bash
cd ElsaQuickstarts.ConsoleApp.HelloWorld
```

Add the following packages:

```bash
dotnet add package Elsa --version 3.3.5
dotnet add package Elsa.Activities.Console --version 2.15.1
```

## The Workflow

Create a new file called `HelloWorld.cs` and add the following code:

```csharp
using Elsa.Activities.Console;
using Elsa.Builders;

namespace ElsaQuickstarts.ConsoleApp.HelloWorld
{
    /// <summary>
    /// A basic workflow with just one WriteLine activity.
    /// </summary>
    public class HelloWorld : IWorkflow
    {
        public void Build(IWorkflowBuilder builder) => builder.WriteLine("Hello World!");
    }
}
```

The above workflow has only one step (a.k.a. activity): `WriteLine`, which writes a line of text to the standard out (the console).

## The Program

Open `Program.cs` and replace its contents with the following:

```csharp
using Elsa.Services;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

namespace ElsaQuickstarts.ConsoleApp.HelloWorld
{
    internal class Program
    {
        private static async Task Main(string[] args)
        {
            // Create the `host` which the application runs within
            IHost host = Host.CreateDefaultBuilder(args)
                .ConfigureServices(services =>
                {
                    services.AddElsaCore(options =>
                    {
                        options.AddConsoleActivities().AddWorkflow<HelloWorld>();
                    });
                })
                .Build();

            // Get a workflow runner.
            var workflowRunner = host.Services.GetRequiredService<IBuildsAndStartsWorkflow>();

            // Run the workflow.
            await workflowRunner.BuildAndStartWorkflowAsync<HelloWorld>();
        }
    }
}
``` 

## Run

Run the program and observe the following output:

```shell
Hello world!
```

Success! You have successfully created and executed an Elsa workflow.

# Next Steps

Now that you've seen how to write and execute a workflow, you might want to learn more about the following:

* How to create more complicated workflows using the Workflow Builder API.
* [How to create a similar workflow using the Workflow Builder API and HTTP activities](./hello-world-http).
* How to create a similar workflow using HTTP activities visually using the Workflow Designer.