# Use Case - Running a bootc container providing Apache HTTP server

In this example, we will build a container image from a Containerfile and we will then use it as a source for a VM.

The Containerfile in the example:

- Updates packages
- Installs tmux and mkpasswd to create a simple user password
- Creates a *bootc-user* user in the image
- Adds the wheel group to sudoers
- Installs [Apache Server](https://httpd.apache.org/)
- Enables the systemd unit for httpd
- Adds a custom index.html

## Building the image

Review the [Containerfile.httpd](Containerfile.httpd) file, that includes all the building steps for the image.

To build the image:

```bash
podman build -f Containerfile.httpd -t centos-bootc-httpd .
```

## Testing the image

You can now test it using:

```bash
podman run -it --name centos-bootc-httpd --hostname centos-bootc-httpd -p 8080:80 centos-bootc-httpd
```

Note: The *"-p 8080:80"* part forwards the container's *http* port to the port 8080 on the host to test that it is working.

The contaienr will now start and a login prompt will appear:

![](./assets/bootc-container.png)

On another terminal tab or in your browser, you can verify that the httpd server is working and serving traffic.

**Terminal**

```bash
 ~ ▓▒░ curl localhost:8080                                                                                                           ░▒▓ ✔  11:59:44
Welcome to the bootc-http instance!
```

**Browser**

![](./assets/browser-test.png)

## Exploring the container

If you are curious, you can easily log-in to the container using the prompt coming from the execution and the **bootc-user/redhat** user and password.

From here, you can verify that:

- The user has sudo privileges

```bash
[bootc-user@centos-bootc-bootc ~]$ sudo su
bash-5.1# whoami
root
```

- There's systemd running
```bash
bash-5.1# systemctl status | more
● centos-bootc-bootc
    State: running
    Units: 233 loaded (incl. loaded aliases)
     Jobs: 0 queued
   Failed: 0 units
    Since: Thu 2024-03-14 11:10:53 UTC; 1min 47s ago
  systemd: 252-27.el9
```

- Apache is loaded as a systemd unit

```bash
bash-5.1# systemctl status httpd
● httpd.service - The Apache HTTP Server
     Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; preset: disabled)
     Active: active (running) since Thu 2024-03-14 11:10:54 UTC; 2min 14s ago
       Docs: man:httpd.service(8)
   Main PID: 94 (httpd)
     Status: "Total requests: 0; Idle/Busy workers 100/0;Requests/sec: 0; Bytes served/sec:   0 B/sec"
      Tasks: 177 (limit: 1638)
     Memory: 15.5M
        CPU: 209ms
     CGroup: /system.slice/httpd.service
             ├─ 94 /usr/sbin/httpd -DFOREGROUND
             ├─124 /usr/sbin/httpd -DFOREGROUND
             ├─126 /usr/sbin/httpd -DFOREGROUND
             ├─127 /usr/sbin/httpd -DFOREGROUND
             └─129 /usr/sbin/httpd -DFOREGROUND
```

