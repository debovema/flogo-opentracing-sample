# Sample for Flogo OpenTracing listener

## Getting started

### Clone this repository

```bash
git clone https://github.com/debovema/flogo-opentracing-sample.git
cd flogo-opentracing-sample
```

### Ensure dependencies

```bash
flogo ensure
```

### Build and run

Build the application:
```bash
flogo build -e
```

The binary is ready to be run in ```./bin``` directory.

## Configure a collector

### Start Zipkin over HTTP

```bash
docker run --name zipkin -d -p 9411:9411 openzipkin/zipkin
```

### Export environment variables

```bash
export FLOGO_OPENTRACING_IMPLEMENTATION=zipkin
export FLOGO_OPENTRACING_TRANSPORT=http
export FLOGO_OPENTRACING_ENDPOINTS=http://127.0.0.1:9411/api/v1/spans
```

If using Docker machine :

```bash
export FLOGO_OPENTRACING_IMPLEMENTATION=zipkin
export FLOGO_OPENTRACING_TRANSPORT=http
export FLOGO_OPENTRACING_ENDPOINTS=http://$(docker-machine ip $(docker-machine active)):9411/api/v1/spans
```

## Test

### Start the executable

```bash
./bin/flogo-opentracing-sample
```

### Call the service

```bash
curl http://127.0.0.1:9233/test
```

### Traces

Results are available at http://127.0.0.1:9411/zipkin/ (change *127.0.0.1* with IP of Docker machine if necessary).


