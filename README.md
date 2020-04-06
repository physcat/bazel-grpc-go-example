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

Update imports for client and server go programs:
```diff
+       pb "github.com/physcat/bazel-grpc-go-example/helloworld"
-       pb "google.golang.org/grpc/examples/helloworld/helloworld"
```
Update the `helloworld/helloworld.proto` file go_package:
```diff
@@ -17,6 +17,7 @@ syntax = "proto3";
 option java_multiple_files = true;
 option java_package = "io.grpc.examples.helloworld";
 option java_outer_classname = "HelloWorldProto";
+option go_package = "github.com/physcat/bazel-grpc-go-example/helloworld";
```

## Step 1:

Use go to find the latest version of the protobuf and grpc tools:
(Here we use the go toolchain to help us later in step 3)

```bash
$ go get -u -v github.com/protocolbuffers/protobuf
$ go get -v -u google.golang.org/grpc
```

## Step 2:

Setup bazel for gazelle

- Create WORKSPACE  and base BUILD.bazel as per [bazel-gazelle](https://github.com/bazelbuild/bazel-gazelle)
- Use gazelle to update WORKSPACE file form go.mod
```bash
bazel run //:gazelle -- update-repos -from_file=go.mod
```

## Step 3:

Check the bazebuild project rules_go under [workspace](https://github.com/bazelbuild/rules_go/blob/master/go/workspace.rst)

We need a repository called `com_google_protobuf` pointing to our new version of `github.com/protocolbuffers/protobuf`.
Gazelle has already added this entry we just need to update it as below:

```diff
 go_repository(
-    name = "com_github_protocolbuffers_protobuf",
+    name = "com_google_protobuf",
     importpath = "github.com/protocolbuffers/protobuf",
     sum = "h1:D7TuYcQlt7ZiIgMqTuBJxvzPklWjyMhqav/sFpnx23Y=",
     version = "v3.11.4+incompatible",
 )
+
+load("@com_google_protobuf//:protobuf_deps.bzl", "protobuf_deps")
+
+protobuf_deps()
```

We don't need anything extra for the grpc because of step 1 gazelle already added everything.

## Step 4:

Now we are ready to let gazelle generate the build files:

```bash
bazel run //:gazelle
```

## Step 5:

All ready just build using bazel. (first build may be slower)

```bash
$ bazel build //greeter_server:greeter_server
$ bazel build //greeter_client:greeter_client
```

And run from two different terminals:
#### Terminal 1
```bash
$ bazel-bin/greeter_server/linux_amd64_stripped/greeter_server
2020/04/06 22:02:19 Received: world
```

#### Terminal 2
```bash
$ bazel-bin/greeter_client/linux_amd64_stripped/greeter_client
2020/04/06 22:02:19 Greeting: Hello world
```

