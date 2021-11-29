# AWS Transit VPC
A solution for creating a transit VPC with Cisco CSR instances (taken from https://github.com/awslabs/aws-transit-vpc). This repo has a working set of generated AWS Lambda packages for deployment to S3 (see https://github.com/camsasuncion/transit-vpc/tree/master/deployment/regional-s3-assets).

# Environment Details
1. Launched an instance of Ubuntu (18.04) as build server with ```ubuntu/images/hvm-ssd/ubuntu-bionic-18.04-amd64-server-20211027```.
3. Installed pip version ```pip 9.0.1 from /usr/lib/python2.7/dist-packages (python 2.7)``` (not pip3). 
4. Used ```python3``` (version 3.6.9) that came with the Ubuntu AMI.
5. To solve the error:
   ```
   START RequestId: 6d9513c4-2cbb-4435-98bc-6d56563e6e55 Version: $LATEST
   [ERROR] Runtime.ImportModuleError: Unable to import module 'transit_vpc_solution_helper/transit-vpc-solution-helper': /lib64/libc.so.6: version `GLIBC_2.18' not    found (required by /var/task/cryptography/hazmat/bindings/_rust.abi3.so
   ```
   we followed the post https://github.com/pyca/cryptography/issues/6391
6. Thus, modified ```build-s3-dist.sh``` from
   ````
   pip3 install . --target ./package
   ````
   to
   ```
   pip install . \
    --platform manylinux2010_x86_64 \
    --implementation cp \
    --python 3.6 \
    --only-binary=:all: --upgrade \
    --target awsbundle \
    cryptography
   ```
 7. Modified relevant AWS CloudFormation mapping templates to use Python3.6 as AWS Lambda runtime.
