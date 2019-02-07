# Lambda Layer Management

This repo provides an example of the Export naming approach and stack hierarchy, as described
in [this medium post](https://medium.com/@geoff.ford_33546/cloudformation-export-management-2f25b2d09443).

## Approach

The aim is to construct a dependency hierarchy of Exports, which provide a readily understandable
naming convention.  The templates do the following:

1. [CRVersioning](https://github.com/geoff-possum/lambda-layer-management/blob/master/crversioning.cform) defines the backing lambda functions for Custom Resources to correctly trigger Lambda
Version increments when a lambda function code or configuration is changed in a stack

2. [CRBase](https://github.com/geoff-possum/lambda-layer-management/blob/master/crbase.cform) defines backing lambda functions for helper Custom Resources (in this case only an S3 URL 
parser).  All lambdas in this template must use correct versioning, hence have a dependency on CRVersioning

3. [Layers](https://github.com/geoff-possum/lambda-layer-management/blob/master/layers.cform) creates a Lambda Layer from a zip file in S3, where the location is provided as an S3 URL.  This 
allows more Layers to be specified in a single template (easing code management), but creates a dependency
on CRBase.  The template specifies that all Lambda Layers are created with a single Runtime, which is then
included in the Export names.  This then simplifies finding and importing the correct layers for Lambda
template designers.

## Example

The [fourth template](https://github.com/geoff-possum/lambda-layer-management/blob/master/lambda_using_example_layer.cform) shows how a Lambda function template designer can then import the correct layer
for their function.  Note that once again, this template also ensures correct Lambda function versioning.

## Licence

This project is released under the MIT license. See [LICENSE](LICENSE) for details.
