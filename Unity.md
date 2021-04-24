Linux execute game
```sh
chmod +x nameOfGame.x86
./nameOfGame.x86 -batchmode
```

IF Code Compile
```c#
#if UNITY_STANDALONE_LINUX || UNITY_EDITOR
    Debug.Log("server");
#endif

if (SystemInfo.deviceName == "domain.server.com")
{
    AutoClickServer(); // 1 host
}
```
