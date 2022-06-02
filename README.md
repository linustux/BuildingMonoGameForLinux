# How to package your game for Linux
(By Linux I am of course referring to [GNU/Linux](https://www.gnu.org/gnu/linux-and-gnu.en.html))

Releasing your game for Linux is super hard, right?
Well, if you have a standard MonoGame setup and happen to be using Desktop OpenGL for rendering your graphics, you're in luck!

Assuming you already have your development environment [setup for targeting Desktop OpenGL](https://docs.monogame.net/articles/getting_started/0_getting_started.html), all it takes are three super easy steps:

## 1 Build your game
As described in [the monogame documentation](https://docs.monogame.net/articles/packaging_games.html#building-and-packaging-for-linux), the command for building a Linux standalone is this:

`$ dotnet publish -c Release -r linux-x64 /p:PublishReadyToRun=false /p:TieredCompilation=false --self-contained`

Note that I use the dollar-sign to indicate that the command does not need (and thus should not be executed with) administrator or superuser privileges. You should _not_ include the '$' when copying the commands.

The command is the same on any operating system. It does of course require you to have the .NET SDK installed, which you already have if you are able to build a release for Windows. If not, [the MonoGame documentation](https://docs.monogame.net/articles/getting_started/0_getting_started.html) has the details.

## 2 Check if your game runs on Linux.

Running the build from the Linux commandline:

`$ ./relative/path/to/YourGame/bin/Release/netcoreapp3.1/linux-x64/publish/YourGame`

or:

`$ /full/path/to/YourGame/bin/Release/netcoreapp3.1/linux-x64/publish/YourGame`

Your game should start as normal. In case you get an error, see below in the [troubleshooting section](https://github.com/linustux/BuildingMonoGameForLinux/tree/main#troubleshooting).

## 3. Archive it as tar.gz
It is recommended to distribute your game as a tar.gz archive as this preserves file permissions.

### Linux / Mac:

The CLI command for creating a tar.gz archive is:

`$ tar -czvf [[filename.tar.gz]] directory`

So for your game this would look somewhat like this:

`$ tar -czvf YourGameName.tar.gz YourGame/bin/Release/netcoreapp3.1/linux-x64/publish`

[Source for Linux](https://www.cyberciti.biz/faq/how-to-create-tar-gz-file-in-linux-using-command-line/), tested on Arch Linux.

[Source for Mac](https://osxdaily.com/2012/04/05/create-tar-gzip/)

See also the [man page of tar](https://man7.org/linux/man-pages/man1/tar.1.html).

### Windows:
- Use 7zip or tar to create a *.tar file out of the folder YourGame\bin\Release\netcoreapp3.1\linux-x64\publish
- Use 7zip or gzip to turn the *.tar into a *.tar.gz file.

[Source](https://stackoverflow.com/questions/10773880/how-to-create-tar-gz-archive-file-in-windows)

## Done
Told you - super easy!

You can now distribute your YourGame.tar.gz archive as a Linux release in exactly the same way as the .zip for Windows release.

## How users can run your game
To run your game, your customers can simply run the following two commands:

```Bash
$ tar -xzvf YourGameName.tar.gz
$ ./publish/YourGameName

```

Of course, if you rename the publish folder to the name of your game before archiving it, that changes the latter command into something a bit more presentable.

You may also want to look into enhancing your release with a .desktop file to allow distros to display your game with desktop menu icons etc.


## Troubleshooting

You may run into the following error when trying to run your Linux build:
```
Process terminated. Couldn't find a valid ICU package installed on the system. Set the configuration flag System.Globalization.Invariant to true if you want to run with no globalization support.
   [...]
```
The easy fix is adding the following property to your YourGameName.csproj file, before building your game again.
```
<PropertyGroup>
    <InvariantGlobalization>true</InvariantGlobalization>
</PropertyGroup>
```

[Source for this fix](https://everythingtech.dev/2021/08/how-to-fix-couldnt-find-a-valid-icu-package-installed-on-the-system-set-the-configuration-flag-system-globalization-invariant-to-true-if-you-want-to-run-with-no-globalization-support/), that I tested on Arch Linux.

