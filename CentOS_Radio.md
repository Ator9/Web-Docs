CentOS 7 - Radio Streaming - Icecast & Ices (OGG)

# 1. Install <a href="http://icecast.org" target="_blank">Icecast</a>
```sh
yum install -y icecast libshout libshout-devel
```

# 2. Copy & Edit Icecast Config File (/etc/icecast.xml)
```xml
<icecast>
    <authentication>
        <source-password>new_password</source-password>
        <admin-password>new_password</admin-password>
    </authentication>
    <hostname>radio.domain.com</hostname>
    <listen-socket>
        <port>8000</port>
    </listen-socket>
    <shoutcast-mount>/bachata.ogg</shoutcast-mount>
</icecast>
```

# 3. Run Icecast in background
```sh
icecast -b -c /path_to/icecast.xml
```
Test Interface URL:
```sh
http://radio.domain.com:8000/
```

# 4. Install <a href="http://www.icecast.org/ices/" target="_blank">Ices</a>
```sh
wget http://downloads.us.xiph.org/releases/ices/ices-2.0.2.tar.gz
tar -zxvf ices-2.0.2.tar.gz
cd ices-2.0.2 ; ./configure
make ; sudo make install
```

# 5. Copy & Edit Ices Config File (ices-playlist.xml)
```xml
<ices>
    <background>1</background>
    <logpath>/var/log/icecast</logpath>
    <stream>
        <metadata>
            <name>Radio Name</name>
            <genre>Radio genre</genre>
            <description>Radio Description!</description>
        </metadata>
        <input>
            <param name="random">1</param>
            <param name="restart-after-reread">1</param>
            <param name="file">/path_to/playlist.txt</param>
        </input>
        <instance>
            <password>new_password</password>
            <mount>/bachata.ogg</mount>
            <encode>
                <nominal-bitrate>64000</nominal-bitrate> <!-- bps. e.g. 64000 for 64 kbps -->
                <samplerate>44100</samplerate>
                <channels>2</channels>
            </encode>
        </instance>
    </stream>
</ices>
```

# 6. Create playlist with directory files
```sh
find /path_to_ogg_files/ -name "*.ogg" > /path_to/playlist.txt
```

# 7. Run Ices
```sh
ices /path_to/ices-playlist.xml
```

# 8. Check your radio :)
Logs
```sh
cat /var/log/icecast/error.log
cat /var/log/icecast/ices.log
```
