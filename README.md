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

## Step 1:

Use go to find the latest version of the protobuf tools:
(Here we use the go toolchain to help us later in step 3)

```bash
go get -v github.com/protocolbuffers/protobuf
```


## Step 2:

Use gazelle to create build files:

- Create WORKSPACE  and base BUILD.bazel as per [bazel-gazelle](https://github.com/bazelbuild/bazel-gazelle)
- Use gazelle to update WORKSPACE file form go.mod
```bash
bazel run //:gazelle -- update-repos -from_file=go.mod
```



