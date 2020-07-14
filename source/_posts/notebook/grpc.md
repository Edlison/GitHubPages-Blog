# gRPC入门

## 下载依赖

`pip install grpc`  
`pip install proto`  
`pip install grpc-tools`  

## 设置接口
在.proto文件中定义接口

## 生成文件
`python -m grpc_tools.protoc -I. --python_out=. --grpc_python_out=. api.proto`

## proto示例
```proto
syntax = "proto3";

// The greeting service definition.
service Greeter {
  // Sends a greeting
  rpc SayHello (HelloRequest) returns (HelloReply) {}
  // Sends another greeting
  rpc SayHelloAgain (HelloRequest) returns (HelloReply) {}
}

// The request message containing the user's name.
message HelloRequest {
  string name = 1;
}

// The response message containing the greetings
message HelloReply {
  string message = 1;
}
```

## server示例
```py
class Greeter(api_pb2_grpc.GreeterServicer):

    def SayHello(self, request, context):
        return api_pb2.HelloReply(message='Hello, %s!' % request.name)

    def SayHelloAgain(self, request, context):
        return api_pb2.HelloReply(message='Hello again, %s!' % request.name)


def serve():
    server = grpc.server(futures.ThreadPoolExecutor(max_workers=4))
    api_pb2_grpc.add_GreeterServicer_to_server(Greeter(), server)
    server.add_insecure_port('localhost:50051')
    server.start()
    try:
        while True:
            time.sleep(60 * 60 * 24)
    except KeyboardInterrupt:
        server.stop(0)


if __name__ == '__main__':
    serve()

```

## client示例
```py
def run():
  channel = grpc.insecure_channel('localhost:50051')
  stub = api_pb2_grpc.GreeterStub(channel)
  response = stub.SayHello(api_pb2.HelloRequest(name='you'))
  print("Greeter client received: " + response.message)
  response = stub.SayHelloAgain(api_pb2.HelloRequest(name='you'))
  print("Greeter client received: " + response.message)


if __name__ == '__main__':
    run()
```