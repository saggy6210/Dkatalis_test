## Dkatalis_test

You can choose to use any scripting language to automate (great if you could do in Ansible/
Terraform). Use the free-tier resources from AWS. Feel free to comment your code and/or put
detailed information in the instructions. The exercise will be 2.5 hours.

### Following is the goals of the exercise:
1. Demonstrate your hands-on skills, you can code for building cloud hosted solution
2. Demonstrate that you can think of other cross-cutting-concerns like security
3. A nice segue to our discussion after you submit the code

### What we are expecting:
1. A link to github repo (or a zip/tarball) with code that accomplishes:
a) Brings up an AWS instance
b) Installs ElasticSearch configured in a way that requires credentials and provides encrypted
communication
c) Demonstrates that it is functioning
2. Instructions with:
a) A short description of your solution describing your choices and why did you make them
b) Resources, if any, that you consulted to arrive at the final solution
c) How long did you spend on the exercise, and if possible, short feedback about the exercise
3. Must use AWS free tier, however, if you’re using any additional services, please mention them
in the instructions
4. ElasticSearch access and communication must be secure

### Bonus if you extend your code to create a cluster of 3 ElasticSearch nodes
Some answers we are looking:
Following are the required details:
1. What did you choose to automate the provisioning and bootstrapping of the instance? Why?

Answer: I choose Terraform for provisioning and bootstrapping of instance because Terraform is IaC, Code reuse, it's plays important role in versioning, its maintain the state of infrastructure so that we can provision the infrastucture in incrementally. It will enable you to rebuild/change and track changes to infrastructure with ease.


