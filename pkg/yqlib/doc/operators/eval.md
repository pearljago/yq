# Eval

Use `eval` to dynamically process an expression - for instance from an environment variable.

`eval` takes a single argument, and evaluates that as a `yq` expression. Any valid expression can be used, beit a path `.a.b.c | select(. == "cat")`, or an update `.a.b.c = "gogo"`.

Tip: This can be useful way parameterise complex scripts.

{% hint style="warning" %}
Note that versions prior to 4.18 require the 'eval/e' command to be specified.&#x20;

`yq e <exp> <file>`
{% endhint %}

## Dynamically evaluate a path
Given a sample.yml file of:
```yaml
pathExp: .a.b[] | select(.name == "cat")
a:
  b:
    - name: dog
    - name: cat
```
then
```bash
yq 'eval(.pathExp)' sample.yml
```
will output
```yaml
name: cat
```

## Dynamically update a path from an environment variable
The env variable can be any valid yq expression.

Given a sample.yml file of:
```yaml
a:
  b:
    - name: dog
    - name: cat
```
then
```bash
pathEnv=".a.b[0].name"  valueEnv="moo" yq 'eval(strenv(pathEnv)) = strenv(valueEnv)' sample.yml
```
will output
```yaml
a:
  b:
    - name: moo
    - name: cat
```

