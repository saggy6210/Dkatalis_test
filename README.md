### Dkatalis_test

You can choose to use any scripting language to automate (great if you could do in Ansible/
Terraform). Use the free-tier resources from AWS. Feel free to comment your code and/or put
detailed information in the instructions. The exercise will be 2.5 hours.

#### Following is the goals of the exercise:
1. Demonstrate your hands-on skills, you can code for building cloud hosted solution
2. Demonstrate that you can think of other cross-cutting-concerns like security
3. A nice segue to our discussion after you submit the code

#### What we are expecting:
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

#### Bonus if you extend your code to create a cluster of 3 ElasticSearch nodes

Some answers we are looking:
Following are the required details:
1. What did you choose to automate the provisioning and bootstrapping of the instance? Why?

Answer: I choose Terraform for provisioning and bootstrapping of instance because Terraform is IaC, Code reuse, it's plays important role in versioning, its maintain the state of infrastructure so that we can provision the infrastucture in incrementally. It will enable you to rebuild/change and track changes to infrastructure with ease.

2. How did you choose to secure ElasticSearch? Why?

Answer: The Elasticsearch security features enable you to secure a cluster/host. You can password-protect your data as well as implement more advanced security measures such as encrypting communications, role-based access control, IP filtering, and auditing. For security concern I choose to secure elasticsearch. 

Following stpes has been followed to enable security on elasticsearch after provisioning elasticsearch host and remote execution of ES installation. 

```sh

$/usr/share/elasticsearch/bin/elasticsearch-certutil  ca
This tool assists you in the generation of X.509 certificates and certificate
signing requests for use with SSL/TLS in the Elastic stack.

The 'ca' mode generates a new 'certificate authority'
This will create a new X.509 certificate and private key that can be used
to sign certificate when running in 'cert' mode.

Use the 'ca-dn' option if you wish to configure the 'distinguished name'
of the certificate authority

By default the 'ca' mode produces a single PKCS#12 output file which holds:
    * The CA certificate
    * The CA's private key

If you elect to generate PEM format certificates (the -pem option), then the output will
be a zip file containing individual files for the CA certificate and private key

Please enter the desired output file [elastic-stack-ca.p12]: 
Enter password for elastic-stack-ca.p12 : 

$/usr/share/elasticsearch/bin/elasticsearch-certutil cert --ca elastic-stack-ca.p12

This tool assists you in the generation of X.509 certificates and certificate
signing requests for use with SSL/TLS in the Elastic stack.

The 'cert' mode generates X.509 certificate and private keys.
    * By default, this generates a single certificate and key for use
       on a single instance.
    * The '-multiple' option will prompt you to enter details for multiple
       instances and will generate a certificate and key for each one
    * The '-in' option allows for the certificate generation to be automated by describing
       the details of each instance in a YAML file

    * An instance is any piece of the Elastic Stack that requires an SSL certificate.
      Depending on your configuration, Elasticsearch, Logstash, Kibana, and Beats
      may all require a certificate and private key.
    * The minimum required value for each instance is a name. This can simply be the
      hostname, which will be used as the Common Name of the certificate. A full
      distinguished name may also be used.
    * A filename value may be required for each instance. This is necessary when the
      name would result in an invalid file or directory name. The name provided here
      is used as the directory name (within the zip) and the prefix for the key and
      certificate files. The filename is required if you are prompted and the name
      is not displayed in the prompt.
    * IP addresses and DNS names are optional. Multiple values can be specified as a
      comma separated string. If no IP addresses or DNS names are provided, you may
      disable hostname verification in your SSL configuration.

    * All certificates generated by this tool will be signed by a certificate authority (CA).
    * The tool can automatically generate a new CA for you, or you can provide your own with the
         -ca or -ca-cert command line options.

By default the 'cert' mode produces a single PKCS#12 output file which holds:
    * The instance certificate
    * The private key for the instance certificate
    * The CA certificate

If you specify any of the following options:
    * -pem (PEM formatted output)
    * -keep-ca-key (retain generated CA key)
    * -multiple (generate multiple certificates)
    * -in (generate certificates from an input file)
then the output will be be a zip file containing individual certificate/key files

Enter password for CA (elastic-stack-ca.p12) : 
Please enter the desired output file [elastic-certificates.p12]: 
Enter password for elastic-certificates.p12 : 

Certificates written to /usr/share/elasticsearch/elastic-certificates.p12

This file should be properly secured as it contains the private key for 
your instance.

This file is a self contained file and can be copied and used 'as is'
For each Elastic product that you wish to configure, you should copy
this '.p12' file to the relevant configuration directory
and then follow the SSL configuration instructions in the product guide.

For client applications, you may only need to copy the CA certificate and
configure the client to trust this certificate.

cp /usr/share/elasticsearch/elastic-certificates.p12 /etc/elasticsearch/
chown root.elasticsearch /etc/elasticsearch/elastic-certificates.p12
chmod 660 /etc/elasticsearch/elastic-certificates.p12

/usr/share/elasticsearch/bin/elasticsearch-certutil  http

## Elasticsearch HTTP Certificate Utility

The 'http' command guides you through the process of generating certificates
for use on the HTTP (Rest) interface for Elasticsearch.

This tool will ask you a number of questions in order to generate the right
set of files for your needs.

## Do you wish to generate a Certificate Signing Request (CSR)?

A CSR is used when you want your certificate to be created by an existing
Certificate Authority (CA) that you do not control (that is, you don't have
access to the keys for that CA). 

If you are in a corporate environment with a central security team, then you
may have an existing Corporate CA that can generate your certificate for you.
Infrastructure within your organisation may already be configured to trust this
CA, so it may be easier for clients to connect to Elasticsearch if you use a
CSR and send that request to the team that controls your CA.

If you choose not to generate a CSR, this tool will generate a new certificate
for you. That certificate will be signed by a CA under your control. This is a
quick and easy way to secure your cluster with TLS, but you will need to
configure all your clients to trust that custom CA.

Generate a CSR? [y/N]N

## Do you have an existing Certificate Authority (CA) key-pair that you wish to use to sign your certificate?

If you have an existing CA certificate and key, then you can use that CA to
sign your new http certificate. This allows you to use the same CA across
multiple Elasticsearch clusters which can make it easier to configure clients,
and may be easier for you to manage.

If you do not have an existing CA, one will be generated for you.

Use an existing CA? [y/N]N
A new Certificate Authority will be generated for you

## CA Generation Options

The generated certificate authority will have the following configuration values.
These values have been selected based on secure defaults.
You should not need to change these values unless you have specific requirements.

Subject DN: CN=Elasticsearch HTTP CA
Validity: 5y
Key Size: 2048

Do you wish to change any of these options? [y/N]N

## CA password

We recommend that you protect your CA private key with a strong password.
If your key does not have a password (or the password can be easily guessed)
then anyone who gets a copy of the key file will be able to generate new certificates
and impersonate your Elasticsearch cluster.

IT IS IMPORTANT THAT YOU REMEMBER THIS PASSWORD AND KEEP IT SECURE

CA password:  [<ENTER> for none]

## How long should your certificates be valid?

Every certificate has an expiry date. When the expiry date is reached clients
will stop trusting your certificate and TLS connections will fail.

Best practice suggests that you should either:
(a) set this to a short duration (90 - 120 days) and have automatic processes
to generate a new certificate before the old one expires, or
(b) set it to a longer duration (3 - 5 years) and then perform a manual update
a few months before it expires.

You may enter the validity period in years (e.g. 3Y), months (e.g. 18M), or days (e.g. 90D)

For how long should your certificate be valid? [5y] 3Y

## Do you wish to generate one certificate per node?

If you have multiple nodes in your cluster, then you may choose to generate a
separate certificate for each of these nodes. Each certificate will have its
own private key, and will be issued for a specific hostname or IP address.

Alternatively, you may wish to generate a single certificate that is valid
across all the hostnames or addresses in your cluster.

If all of your nodes will be accessed through a single domain
(e.g. node01.es.example.com, node02.es.example.com, etc) then you may find it
simpler to generate one certificate with a wildcard hostname (*.es.example.com)
and use that across all of your nodes.

However, if you do not have a common domain name, and you expect to add
additional nodes to your cluster in the future, then you should generate a
certificate per node so that you can more easily generate new certificates when
you provision new nodes.

Generate a certificate per node? [y/N]N

## Which hostnames will be used to connect to your nodes?

These hostnames will be added as "DNS" names in the "Subject Alternative Name"
(SAN) field in your certificate.

You should list every hostname and variant that people will use to connect to
your cluster over http.
Do not list IP addresses here, you will be asked to enter them later.

If you wish to use a wildcard certificate (for example *.es.example.com) you
can enter that here.

Enter all the hostnames that you need, one per line.
When you are done, press <ENTER> once more to move on to the next step.

13.233.103.81

You entered the following hostnames.

 - 13.233.103.81

Is this correct [Y/n]Y

## Which IP addresses will be used to connect to your nodes?

If your clients will ever connect to your nodes by numeric IP address, then you
can list these as valid IP "Subject Alternative Name" (SAN) fields in your
certificate.

If you do not have fixed IP addresses, or not wish to support direct IP access
to your cluster then you can just press <ENTER> to skip this step.

Enter all the IP addresses that you need, one per line.
When you are done, press <ENTER> once more to move on to the next step.

13.233.103.81

You entered the following IP addresses.

 - 13.233.103.81

Is this correct [Y/n]Y

## Other certificate options

The generated certificate will have the following additional configuration
values. These values have been selected based on a combination of the
information you have provided above and secure defaults. You should not need to
change these values unless you have specific requirements.

Key Name: 13.233.103.81
Subject DN: CN=13, DC=234, DC=67, DC=113
Key Size: 2048

Do you wish to change any of these options? [y/N]N

## What password do you want for your private key(s)?

Your private key(s) will be stored in a PKCS#12 keystore file named "http.p12".
This type of keystore is always password protected, but it is possible to use a
blank password.

If you wish to use a blank password, simply press <enter> at the prompt below.
Provide a password for the "http.p12" file:  [<ENTER> for none]

## Where should we save the generated files?

A number of files will be generated including your private key(s),
public certificate(s), and sample configuration options for Elastic Stack products.

These files will be included in a single zip archive.

What filename should be used for the output zip file? [/usr/share/elasticsearch/elasticsearch-ssl-http.zip] 

Zip file written to /usr/share/elasticsearch/elasticsearch-ssl-http.zip

cd /usr/share/elasticsearch
unzip elasticsearch-ssl-http.zip
cp  /usr/share/elasticsearch/elasticsearch/http.p12 /etc/elasticsearch/
chown root.elasticsearch /etc/elasticsearch/http.p12
chmod 660 /etc/elasticsearch/http.p12
```

- Generate Password for elasticsearch:

```sh
/usr/share/elasticsearch/bin/elasticsearch-setup-passwords auto -u https://13.233.103.81:9200
```

> Screenshots: 

https://github.com/saggy6210/Dkatalis_test/blob/main/Screenshot%20from%202021-03-11%2000-19-26.png

https://github.com/saggy6210/Dkatalis_test/blob/main/Screenshot%20from%202021-03-11%2000-18-54.png


3. How would you monitor this instance? What metrics would you monitor?

Answer: 
Using Kibana or Grafana, we can monitor this instance by configuring following metrics:
Instance Health – Nodes and Shards
Search Performance – Request Rate
Indexing Performance – Refresh Times
Instance Health – Memory Usage
Instance Health – Disk I/O
Instance Health – CPU
Other data based metrics - Oprational

4. Could you extend your solution to launch a secure cluster of ElasticSearch nodes? What would need to change to support this use case?

Answer: 
Yes, I can extend to launch a secure cluster of ElasticSearch nodes. Only has to create (N) number of instances and cluster configuration in /etc/elasticsearch/elasticsearch.yml

5. Could you extend your solution to replace a running ElasticSearch instance with little or no downtime? How?

Answer: Yes,
Run the migration plugin on your production cluster to find data incompatibility early in the migration process.
Test migration in a dev environment before migration your production cluster.
Always take a snapshot(backup of your data before migration.
Automate the ElasticSearch cluster provisioning using terraform or any other tool.

6. Was it a priority to make your code well structured, extensible, and reusable?

Answer: 
Yes, that's why I have used Terraform for infrastructure provisioing, and well structured code, extensible and resusable are the features of the terraform. 

7. What sacrifices did you make due to time?

Answer: 
Sacrifies always based on the opportunity cost, I spend have spent almost 3 hrs time on this assignment,it was a great learning for me. 

Please share the feedback!
