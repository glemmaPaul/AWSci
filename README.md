# AWS Continuous Integration

## Description
AWSCI is a Python module that enables you to spin up an instance on EC2 to run your tests on. This together with a Yaml interperter which gives you the possibility to add certain commands to the server. The builds are supported with Docker (Docker Hub will be supported soon).

## Installation
The installation is pretty straight forward, just easily with `pip`:
```bash
pip install awsci
```

## Usage
There are two ways to use the client. The first way is using it through a Yaml interface. Our Yaml structure is heavily inspired on other continues integration platforms.

### Using it with Yaml
Here's an example of using the client through an yAml:
```yaml
box: python
aws_access:
    key: "<YOUR AMAZON KEY>"
    secret: "<YOUR SECRET AMAZON KEY>"
install: 
  - "./testsinstall.sh"
  - "pip install -r requirements/test.txt"
script: 
  - python ./manage.py test
after_success:
  - "python deploy_script.py"
```

#### Events:
- `box`: The docker box, for more info on docker boxes: https://hub.docker.com/ (Default: `python`)
- `aws_access`: Your AWS account info
- `install`: Installation scripts in list, will be run sequentely.
- `script`: The actually script to run, your tests, your whatsoever!
- `after_success`: Anything you want to do afterwards when successful? Add them there

Afterwards you can load the Yaml in our `CIInstance` class:
```python
ci = CIInstance.from_yaml('awsci.yaml', "")
ci.run_instance()
```

### Using it manually
Here an example to do it manually

```python
ci = CIInstance("<YOUR AMAZON KEY>", "<YOUR SECRET AMAZON KEY>")

# Set the box
ci.set_box("python")

# Add installation arguments
ci.install.add("./testsinstall.sh")
ci.install.add("pip install -r requirements/test.txt")

# Add script
ci.script.add("python ./manage.py test")

# Add the after success
ci.after_success.add("python deploy_script.py")

ci.run_instance()
```
