# Sample for Flogo OpenTracing model

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

### Patch flogo-contrib and flogo-lib

**IMPORTANT**: This model requires some little updates in flogo-contrib and flogo-lib which are not yet merged into
TIBCOSoftware repositories. **This section will be removed after the merge**.
A script is provided to perform the operation.

In the directory of the Flogo project (with a *flogo.json* file), run:

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/debovema/flogo-contrib-models/master/opentracing/patch-vendor.sh)"
```

### Build and run

Finally, build the application:
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

## Development

Here's another method to patch vendor which is more suitable for development.

For Cygwin/Babun environment (Windows), first define:
```bash
CYGWIN=winsymlinks:nativestrict
```

Link required repository to the ones in your $GOPATH:

```bash
rm -f ./src/flogo-opentracing-sample/vendor/github.com/debovema/flogo-contrib-models
rm -f ./src/flogo-opentracing-sample/vendor/github.com/TIBCOSoftware/flogo-contrib
rm -f ./src/flogo-opentracing-sample/vendor/github.com/TIBCOSoftware/flogo-lib

ln -s $GOPATH/src/github.com/debovema/flogo-contrib-models ./src/flogo-opentracing-sample/vendor/github.com/debovema/flogo-contrib-models
ln -s $GOPATH/src/github.com/debovema/flogo-contrib-models ./src/flogo-opentracing-sample/vendor/github.com/TIBCOSoftware/flogo-contrib
ln -s $GOPATH/src/github.com/debovema/flogo-contrib-models ./src/flogo-opentracing-sample/vendor/github.com/TIBCOSoftware/flogo-lib
```

Change the TIBCOSoftware contribs to the right remote and branch:
```bash
git -C $GOPATH/src/github.com/TIBCOSoftware/flogo-contrib remote set-url origin https://github.com/debovema/flogo-contrib.git
git -C $GOPATH/src/github.com/TIBCOSoftware/flogo-contrib pull origin working-data-between-flow-and-activities

git -C $GOPATH/src/github.com/TIBCOSoftware/flogo-lib remote set-url origin https://github.com/debovema/flogo-lib.git
git -C $GOPATH/src/github.com/TIBCOSoftware/flogo-lib pull origin working-data-between-flow-and-activities
```
