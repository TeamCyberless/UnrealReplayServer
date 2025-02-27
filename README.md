# Unreal Engine HTTP Replay Server
This is a sample implementation of an Unreal Engine 5 HTTP Replay Streaming Sample Server using ASP.NET 8

## What is HTTP Replay Streaming?
Unreal Engine 5 allows you to record and play replays of your gaming sessions. There are various factories for this (in-memory of the Client, to file on Client, to SaveGame on Client) and the HTTP streaming server is one of them. This project implements such a 
server, which allows users to record their playback to a server and let other people rewatch the same replay from their own
computer later using a shared session name.  

> Note:  
> This doesn't mean that you can use this to record replays of any Unreal Engine game out there.  
> This project is for developers creating their own UE game and wanting to provide an Internet-based replay service.  

## Usage
In your Unreal projects DefaultEngine.ini you have to set the HttpNetworkReplayStreaming class as the replay streaming factory and 
set the correct ServerURL for your server.

The following works for me in UE 5.3.1:
```
[NetworkReplayStreaming]
DefaultFactoryName=HttpNetworkReplayStreaming

[HttpNetworkReplayStreaming]
ServerURL="http://127.0.0.1:5000/"
```
(replace the ServerURL with whatever you have, of course)  

Then just run this server, open up a level in Unreal Editor, play in Editor (or standalone) and use the 
Engine console to record a replay:
`demorec test`

Then stop recording with `demostop`.

To play the recording use `demoplay test`.

[See here for more information](https://docs.unrealengine.com/en-US/TestingAndOptimization/ReplaySystem/index.html)
(The C++ functions mentioned on that page may also provide you with more control over the process).

You should record at least 10 or more seconds, otherwise you won't see much when you replay the recording (because it's already 
finished before the level has come up properly).

## Enhancements

The two interfaces `IEventDatabase` and `ISessionDatabase` have a default in-memory implementation. You can write your own 
implementation of the interfaces and set them up in `Startup.cs::ConfigureServices`.  

The interfaces expose all their methods as `async`, so it should be easily possible to implement them using EF core or any
other database framework for persistency.  

I did not pay attention to optimization. There's probably a lot that could be done in this regard ;-)

Live replay isn't tested and probably doesn't work, because I return `false` whenever "is live" is mentioned somewhere. 
However, it shouldn't be hard to implement, because it's likely mostly about setting "is live" correctly and managing timestamps 
more cleanly.

## License

The MIT License (MIT)

Copyright (c) 2021 Henning Thoele

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation 
files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, 
modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the 
Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE
WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS 
OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR 
OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
