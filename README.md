# Bazel protobuf and grpc go example

## Step 0:

Start with the example files form: [grpc-go](https://github.com/grpc/grpc-go)

- examples/helloworld/greeter_client/main.go
- examples/helloworld/greeter_server/main.go
- examples/helloworld/helloworld/helloworld.proto


Setup your go modules and update imports to reflect changes:

```bash
$ go mod init github.com/physcat/bazel-grpc-go-example
```

Update imports:
```diff
+       pb "github.com/physcat/bazel-grpc-go-example/helloworld"
-       pb "google.golang.org/grpc/examples/helloworld/helloworld"
```


