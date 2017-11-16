---
uid: getting-started-part1
---

# Part 1

In this section, we will set up a new project, create a Window to draw into, and create a Veldrid GraphicsDevice. We will also set up a very simple skeleton for the other sections to fill in.

## Create a new Project

This walkthrough assumes you are using the .NET Core toolchain, although the same code could be run on .NET Framework or Mono.

Create a new console application by running `dotnet new console`, or by using a "New Project" dialogue in Visual Studio or other IDE. Then, add a reference to two Veldrid NuGet packages:

```XML
<PackageReference Include="Veldrid" Version="4.0.0-gaa5ff5bed4" />
<PackageReference Include="Veldrid.StartupUtilities" Version="4.0.0-gaa5ff5bed4" />
```

## Creating a Window

Veldrid itself does not care what framework or library you use to manage your window or render view -- it is flexible enough to work with many different systems. The `VeldridStartup` static class (from Veldrid.StartupUtilities) includes a number of helper functions intended to make Window and GraphicsDevice creation easier for common scenarios, which will be used here.

First, we will create a Window. Inside the `Main()` method, let's add some code.

```C#
WindowCreateInfo windowCI = new WindowCreateInfo()
{
    X = 100,
    Y = 100,
    WindowWidth = 960,
    WindowHeight = 540,
    WindowTitle = "Veldrid Tutorial"
};
Sdl2Window window = VeldridStartup.CreateWindow(ref windowCI);
```

This creates a new Sdl2Window with some basic properties. Next, we will create a GraphicsDevice attached to it. This will let us issue graphics commands into the window. Create a new [GraphicsDevice](xref:Veldrid.GraphicsDevice) field called `_graphicsDevice`. We will use another `VeldridStartup` helper method to create it:

```C#
GraphicsDeviceCreateInfo gdCI = new GraphicsDeviceCreateInfo();
_graphicsDevice = VeldridStartup.CreateGraphicsDevice(ref gdCI, window);
```

`GraphicsDeviceCreateInfo` has two properties that we could set: a boolean controlling whether the device will report debug information (`DebugDevice`), and a [GraphicsBackend](xref:Veldrid.GraphicsBackend) identifying a preferred graphics API to use (Vulkan, Direct3D 11, or OpenGL). When unspecified, the default backend for the platform will be chosen. We will leave the default.

Next, let's add in a very basic application loop that keeps the window running. At the bottom of `Main()`:

```C#
while (window.Exists)
{
    window.PumpEvents();
}
```

If we run this code, we will not see anything terribly interesting. A blank window will appear, which can be moved around, resized, and closed, but it will not display anything.

In the next section, we will create some Veldrid graphics resources which we will need to render a basic multi-colored quad. In the third section, we will draw the quad and perform resource and application cleanup.

[Next: Part 2](xref:getting-started-part2)