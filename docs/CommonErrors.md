# Common Errors
## Nuclio

### Reading JSON without authentication
#### Description
```
JSON parser expected column....
```
#### Solution
Add authentication.

### Cannot connect to the Docker daemon
#### Description
```
Call stack:
stdout:
Client: Docker Engine - Community
 Version:           18.09.6
 API version:       1.39
 Go version:        go1.10.8
 Git commit:        481bc77
 Built:             Sat May  4 02:33:34 2019
 OS/Arch:           linux/amd64
 Experimental:      false
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
stderr:
    /nuclio/pkg/cmdrunner/cmdrunner.go:124
No docker client found
    /nuclio/pkg/dockerclient/shell.go:65
Failed to create docker client
    ...//nuclio/pkg/processor/build/runtime/runtime.go:105
Failed to create abstract runtime
    .../pkg/processor/build/runtime/python/factory.go:36
Failed to create runtime
    /nuclio/pkg/processor/build/builder.go:838
Failed create runtime
    /nuclio/pkg/processor/build/builder.go:216
```
#### Solution
Restart `Nuclio` service.