# Ansible AWS Route53
An Ansible role that will create a record in Route53

## Requirements
- AWS credentials and the correct permissions to create the resources
- An existing Zone

## Dependencies
None

## Outputs
The role provides the following outputs:

`dns_records`: This fact can be used in a further play 

## Role Variables

The variables uses in this role are:

| Variable Name | Required | Description | 
|----|----|----|
| `route53_zone`| **Yes** | The DNS zone to modify |
| `route53_record` | **Yes** | The full DNS record to create or delete |
| `route53_record_value` | **Yes** | The new value when creating a DNS record. When deleting a record all values for the record must be specified or Route53 will not delete it. |
| `route53_hosted_zone_id` | **Yes** | The Hosted Zone ID of the DNS zone to modify |
| `route53_private_zone` | Optional | If set to yes, the private zone matching the requested name within the domain will be used if there are both public and private zones. The default is to use the public zone. <br> - Default `no` |
| `route53_record_ttl` | Optional | The TTL to give the new record. <br> - Default `3600` |
| `route53_record_type` | Optional | The type of DNS record to create <br> - Default `A` |
| `route53_wait` | Optional | Wait until the changes have been replicated to all Amazon Route 53 DNS servers. <br> - Default `no` |
| `route53_wait_timeout` | Optional | How long to wait for the changes to be replicated, in seconds. <br> - Default `300` |


## Example Playbook

### Download dependencies

#### Create requirements file

Create a `requirements.yml` file with the following contents

```
- src: https://github.com/maishsk/aws-route53
  version: master
```

#### Download dependencies
Run the following command:
`ansible-galaxy install -r requirements.yml --force -p .`

### Create playbook
Create a `main.yaml` file with the following contents:
```
---
- name: Create DNS Record
  hosts: localhost
  connection: local
  gather_facts: false
  roles:
    - ansible-aws-route53
```

Create a `vars/vars.yml` with the content similar to:

```
route53_zone: foo.bar.
route53_record: helloworld.foo.bar.
route53_record_value: 1.1.1.1
route53_hosted_zone_id: ABCDEFGHIJKLMNOP
```

### Running the playbook

To create the DNS Record

`ansible-playbook main.yml -e @vars.yml --tags create`

To remove the DNS record 

`ansible-playbook main.yml -e @vars.yml --tags rollback`

## License

BSD

## Author Information
This role was created by [Maish Saidel-Keesing](https://www.maishsk.com/), author of [The Cloud Walkabout](http://cloudwalkabout.com/).