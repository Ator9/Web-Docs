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

```xml
<icecast>

    <!--<master-server>127.0.0.1</master-server>-->
    <!--<master-server-port>8001</master-server-port>-->
    <!--<master-update-interval>120</master-update-interval>-->
    <!--<master-password>hackme</master-password>-->

    <!-- setting this makes all relays on-demand unless overridden, this is
         useful for master relays which do not have <relay> definitions here.
         The default is 0 -->
    <!--<relays-on-demand>1</relays-on-demand>-->

    <!--
    <relay>
        <server>127.0.0.1</server>
        <port>8001</port>
        <mount>/example.ogg</mount>
        <local-mount>/different.ogg</local-mount>
        <on-demand>0</on-demand>

        <relay-shoutcast-metadata>0</relay-shoutcast-metadata>
    </relay>
    -->

    <!-- Only define a <mount> section if you want to use advanced options,
         like alternative usernames or passwords
    <mount>
        <mount-name>/example-complex.ogg</mount-name>

        <username>othersource</username>
        <password>hackmemore</password>

        <max-listeners>1</max-listeners>
        <dump-file>/tmp/dump-example1.ogg</dump-file>
        <burst-size>65536</burst-size>
        <fallback-mount>/example2.ogg</fallback-mount>
        <fallback-override>1</fallback-override>
        <fallback-when-full>1</fallback-when-full>
        <intro>/example_intro.ogg</intro>
        <hidden>1</hidden>
        <no-yp>1</no-yp>
        <authentication type="htpasswd">
                <option name="filename" value="myauth"/>
                <option name="allow_duplicate_users" value="0"/>
        </authentication>
        <on-connect>/home/icecast/bin/stream-start</on-connect>
        <on-disconnect>/home/icecast/bin/stream-stop</on-disconnect>
    </mount>

    <mount>
        <mount-name>/auth_example.ogg</mount-name>
        <authentication type="url">
            <option name="mount_add"       value="http://myauthserver.net/notify_mount.php"/>
            <option name="mount_remove"    value="http://myauthserver.net/notify_mount.php"/>
            <option name="listener_add"    value="http://myauthserver.net/notify_listener.php"/>
            <option name="listener_remove" value="http://myauthserver.net/notify_listener.php"/>
        </authentication>
    </mount>

    -->

    <fileserve>1</fileserve>

    <paths>
        <!-- basedir is only used if chroot is enabled -->
        <basedir>/usr/share/icecast</basedir>

        <!-- Note that if <chroot> is turned on below, these paths must both
             be relative to the new root, not the original root -->
        <logdir>/var/log/icecast</logdir>
        <webroot>/usr/share/icecast/web</webroot>
        <adminroot>/usr/share/icecast/admin</adminroot>
        <pidfile>/var/run/icecast/icecast.pid</pidfile>

        <!-- Aliases: treat requests for 'source' path as being for 'dest' path
             May be made specific to a port or bound address using the "port"
             and "bind-address" attributes.
          -->
        <!--
        <alias source="/foo" dest="/bar"/>
          -->
        <!-- Aliases: can also be used for simple redirections as well,
             this example will redirect all requests for http://server:port/ to
             the status page
          -->
        <alias source="/" dest="/status.xsl"/>
    </paths>
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
    <!-- run in background -->
    <background>0</background>
    <!-- where logs, etc go. -->
    <logpath>/var/log/ices</logpath>
    <logfile>ices.log</logfile>
    <!-- 1=error,2=warn,3=info,4=debug -->
    <loglevel>4</loglevel>
    <!-- set this to 1 to log to the console instead of to the file above -->
    <consolelog>0</consolelog>

    <!-- optional filename to write process id to -->
    <!-- <pidfile>/home/ices/ices.pid</pidfile> -->

    <stream>
        <!-- metadata used for stream listing (not currently used) -->
        <metadata>
            <name>Example stream name</name>
            <genre>Example genre</genre>
            <description>A short description of your stream</description>
        </metadata>

        <!-- input module

            The module used here is the playlist module - it has
            'submodules' for different types of playlist. There are
            two currently implemented, 'basic', which is a simple
            file-based playlist, and 'script' which invokes a command
            to returns a filename to start playing. -->

        <input>
            <module>playlist</module>
            <param name="type">basic</param>
            <param name="file">playlist.txt</param>
            <!-- random play -->
            <param name="random">0</param>
            <!-- if the playlist get updated that start at the beginning -->
            <param name="restart-after-reread">0</param>
            <!-- if set to 1 , plays once through, then exits. -->
            <param name="once">0</param>
        </input>

                <!-- Stream instance
            You may have one or more instances here. This allows you to
            send the same input data to one or more servers (or to different
            mountpoints on the same server). Each of them can have different
            parameters. This is primarily useful for a) relaying to multiple
            independent servers, and b) encoding/reencoding to multiple
            bitrates.
            If one instance fails (for example, the associated server goes
            down, etc), the others will continue to function correctly.
            This example defines two instances as two mountpoints on the
            same server.  -->
        <instance>
            <!-- Server details:
                You define hostname and port for the server here, along with
                the source password and mountpoint.  -->
            <hostname>localhost</hostname>
            <port>8000</port>
            <password>hackme</password>
            <mount>/example1.ogg</mount>

            <!-- Reconnect parameters:
                When something goes wrong (e.g. the server crashes, or the
                network drops) and ices disconnects from the server, these
                control how often it tries to reconnect, and how many times
                it tries to reconnect. Delay is in seconds.
                If you set reconnectattempts to -1, it will continue
                indefinately. Suggest setting reconnectdelay to a large value
                if you do this.
            -->
            <reconnectdelay>2</reconnectdelay>
            <reconnectattempts>5</reconnectattempts>

            <!-- maxqueuelength:
                This describes how long the internal data queues may be. This
                basically lets you control how much data gets buffered before
                ices decides it can't send to the server fast enough, and
                either shuts down or flushes the queue (dropping the data)
                and continues.
                For advanced users only.
            -->
            <maxqueuelength>80</maxqueuelength>

            <!-- Live encoding/reencoding:
                Currrently, the parameters given here for encoding MUST
                match the input data for channels and sample rate. That
                restriction will be relaxed in the future.
                Remove this section if you don't want your files getting reencoded.
            -->
            <encode>
                <nominal-bitrate>64000</nominal-bitrate> <!-- bps. e.g. 64000 for 64 kbps -->
                <samplerate>44100</samplerate>
                <channels>2</channels>
            </encode>
        </instance>

        </stream>
</ices>
```

# 6. Run Ices
```sh
ices /path_to/ices-playlist.xml
```

