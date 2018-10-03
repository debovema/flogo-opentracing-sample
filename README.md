# Sample for Flogo OpenTracing model

## Clone this repository

```bash
git clone https://github.com/debovema/flogo-opentracing-sample.git
cd flogo-opentracing-sample
```

## Ensure dependencies

```bash
flogo ensure
```

## Patch flogo-contrib and flogo-lib

**IMPORTANT**: This model requires some little updates in flogo-contrib and flogo-lib which are not yet merged into
TIBCOSoftware repositories. **This section will be removed after the merge**.
A script is provided to perform the operation.

In the directory of the Flogo project (with a *flogo.json* file), run:

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/debovema/flogo-contrib-models/master/opentracing/patch-vendor.sh)"
```

## Build and run

Finally, build the application:
```bash
flogo build -e
```

The binary is ready to be run in ```./bin``` directory.
