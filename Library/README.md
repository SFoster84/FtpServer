# Zhaobang.FtpServer
A FTP library for .NET Standard 1.4.

Build Status: [![Build status](https://ci.appveyor.com/api/projects/status/f1nxe8ru98x321s3/branch/master?svg=true)](https://ci.appveyor.com/project/taoyouh/ftpserver/branch/master)

Get stable release: [![Get stable release](https://img.shields.io/nuget/v/Zhaobang.FtpServer.svg)](https://www.nuget.org/packages/Zhaobang.FtpServer/)

Get testing release: [![Get testing release](https://img.shields.io/myget/zhaobang-ftpserver-nightly/vpre/Zhaobang.FtpServer.svg)](https://www.myget.org/feed/zhaobang-ftpserver-nightly/package/nuget/Zhaobang.FtpServer)

## Simple Usage
The server allows anonymous login, and users can read and write in a public directory.
### To start the server
```
// using System.Net;
// using System.Threading;
// using Zhaobang.FtpServer;

var endPoint = new IPEndPoint(IPAddress.Any, 21);
// To accept IPv6 connection, replace "IPAddress.Any" with "IPAddress.IPv6Any"
// You need 2 FtpServer instances to accept both IPv4 and IPv6 connectins

var baseDirectory = "C:\\FtpServer";
var server = new FtpServer(endPoint, baseDirectory);

var cancelSource = new CancellationTokenSource();
var runResult = server.RunAsync(cancelSource.Token);
```
### To stop the server
```
cancelSource.Cancel(); // Stop accepting new clients
await runResult; // Wait until last client quits
```

## Customization
The server allows developer to use customized authentication, file provider, and data connection.

Implement Zhaobang.FtpServer.File.IFileProviderFactory to use custom file system. The default one is SimpleFileProvider, which allow all users to read and write in a single directory.

Implement Zhaobang.FtpServer.Connections.IDataConnectionFactory to use custom data connection, (for example, establish data connection from another server). The default one is LocalDataConnectionFactory, which establish data connection from local server.

Implement Zhaobang.FtpServer.Authenticate.IAuthenticator to use custom authentication. The default one is AnonymousAuthenticator, which only allows anonymous logins.

Use the following to start your customized server:
```
var server = new FtpServer(
    localEP,
    new MyFileProviderFactory(),
    new MyDataConnectionFactory(),
    new MyAuthenticator()
);
// the remaining is same as simple use
```