
A variable is set at runtime, allowing us to vary Terraform's behaviour. 
- Therefore, if Terraform were a function, a variable would be an input to the function.
- note: not to be confused with `locals`, which themselves are actually more like variables as used in general programming

```hcl
variable "bucket_name" {
    type = string
    # describe what this variable is used for
    description = "the name of the bucket we are creating"
    default = "default_bucket_name"
}

resource "aws_s3_bucket" "bucket" {
    bucket = var.bucket_name
}
```

Variables can be more complex too:
```tfvars
instance_map = {
    dev = "t3.small"
    test = "t3.medium"
    prod = "t3.large"
}

environment_type = "dev"
```

And referenced like:
```hcl
variable "instance_map" {
    type = map(string)
}
variable "environment_type" {}

output "selected_instance" {
    value = var.instance_map[var.environment_type]
}
```

#### Types
- `string`
- `bool`
- `number`
- `list(<TYPE>)`
- `set(<TYPE>)`
    - each value is unique
- `map(<TYPE>)`
- `object()`
    - like a map, but values can be different types
- `tuple([<TYPE>, …])`
    - number of values and order is preserved
- `any`
    - unlike `any` type from [[Typescript|ts]]; this `any` allows Terraform to infer based on the actual value.

#### Providing variables (4 ways)
1. When we run `terraform init` and `terraform apply`, we will be prompted to provide a value for the variable(s).

2. pass the value with:
```sh
terraform apply -var bucket_name=my_bucket
```

3. `export` environment variables in the terminal prefixed with `TF_VAR_`:
```sh
export TF_VAR_bucket_name=my_bucket
```

4. create a `terraform.tfvars` file (or `<ANYNAME>.auto.tfvars`):
```tfvars
bucket_name = "my_bucket"
```
