---
title: AWS Cloud Formation
layout: post
tags: [aws, cda, cloudformation]
date: 2020-09-14
---

### Cloud Formation: Introduction

- infrastracture as code, easy to recreate
-  declarative way, pseudo code
-  no manual, versioned by git, changes by code
- can track cost of a stack, saving strategy creating,stoping stack
- no need to orchestrace create, separation of concerns (VPC, network, app)
- using templates online, also useful for generate documentation
- template componentents: resources(mandatory), parametars, mapping, outputs, condiditionals, metadata
- update and version of templates
- YAML format, key pair values, nested values, arrays (with "-") or list, multi line string (with like logical or just one line "//|")  



## AWS reference types
- https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html
- no dynaminc amount of resources, everything declared
- aws service is supported, only a few not yet
- reference of param or resource exp: !Ref MyVPC . concept of pseudo params
- mappings are fixed variables withing template. differentiate between environments, regions, types. values are hardcoded
- !FindInMap [ MapName, ToplevleKey, SecondLevelKey] - returns a value from specific key
- outputs are optional, we can import into other stacks if exported. cross stack colaboration. Export: Name: ValueName . !ImportValue ValueName .
- conditionals - conditoinals on env, regions
```yaml
Conditions:
  CreateProdResources: !Equals [!Ref EnvType, prod ]
``` 
- Intrisic functions: Ref, Fn:GetAtt, Fn:FindInMap, Fn:ImportValue, Fn:Join, Fn:Sub, conditions func (Fn::If, Fn:Not, Fn:Equals...)
- Fn::Ref - parametars, resources. !Ref . like get of id.
- Fn:GetAtt- get attributes for instances. !GetAtt EC2Instance.Availability
- Fn:FindInMap -  !FindInMap [ MapName, ToplevleKey, SecondLevelKey] - returns a value from specific key
- Fn:ImportValue - !ImportValue SSHSecurityGroup
- Fn:Join - join values with a delimiter !Join [":", [a,b,c]}
- Fn:Sub or !Sub - used to substitute variable from text. customize templates. 
- Rollbacks: default everything gets rollback. Update fails- gets back to working state. 
- CloudFormation - ChangeSet is applied after is reviewed to current stack.
- Cross vs NestedStack - Cross helpful when there are different lifecycles, with use of Outputs Exports and import values. Passing values into stacks.
Nested is helpful for reuse. Its not shared, exp. reuse of load balancer.
- StackSets- create/update/delete stacks acorss multiple accounts and regions with one operation. 

## questions
- Before being used by CloudFormation, your templates must be uploaded to AWS S3.
- Export output name must be unique inside region
- 
