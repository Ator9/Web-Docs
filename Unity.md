Linux execute game
```sh
chmod +x nameOfGame.x86
./nameOfGame.x86 -batchmode -nographics
```

IF Code Compile
```c#
#if HEADLESS || UNITY_STANDALONE_LINUX || UNITY_EDITOR
    Debug.Log("server");
#endif

if (SystemInfo.deviceName == "domain.server.com")
{
    AutoClickServer(); // 1 host
}
```
