Linux execute game
```sh
chmod +x nameOfGame.x86
./nameOfGame.x86 -batchmode
```

Code Build Exclusion
```c#
#if UNITY_STANDALONE_LINUX || UNITY_EDITOR
    Debug.Log("server");
#endif
```

Server check if
```c#
if (SystemInfo.deviceName == "domain.server.com")
{
    AutoClickServer(); // 1 host
}
```
