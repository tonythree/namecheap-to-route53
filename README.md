# Domain transfer from namecheap to Route53
How to successfully migrate your namecheap records to route53 using cli53 :)

## Get the Zone file
Login in Namecheap, and ask Namecheap support to send you the zone file of the domain you're transfering. This is a file containing all the records for your specific domain (A, TXT, MX, CNAME...). See zonefile.txt as an example.

## Format the Zone file
Namecheap send it to me in .docx format. Copy the text and paste it in a zonefile.txt file. Some other stuff you need to do:
- Remove whitespaces in the beginning of all lines in zone file.
- Quote all TXT record values, for example:
```
@    1799   IN     TXT   example=11111?
```
should be:
```
@    1799   IN     TXT   "example=11111?"
``` 

## Install cli53
- Install cli53. See github repo: https://github.com/barnybug/cli53/#installation
cli53 provides import and export from BIND format and simple command line management of Route 53 domains.
For mac users:
```
brew install cli53
```
- cli53 will automatically get your aws credentials. Make sure you are using the correct ones:
```
aws configure
```

- Check cli53 installation by listing the current hosted zones you have in route53. Also shows the record count.
```
cli53 list
```

## Create Route53 hosted zone
- Create a new Route53 hosted zone for your domain. You can do it manually in the aws console or using cli53.
To create it manually, go to: https://console.aws.amazon.com/route53/v2/hostedzones#
To create it the fancy way using cli53:
```
cli53 create example.com --comment 'my first zone'
```
Replace "example.com" with your domain.

Now, you should see the new created hosted zone by doing:
```
cli53 list
```

## Import your Zone file in route53 hosted zone
Using cli53, import your zonefile.txt with 1 line of code:
```
cli53 import example.com --file zonefile.txt
```
Replace "example.com" with your domain.
Now check again, you should see a higher number of records in your hosted zone:
```
cli53 list
```

## Make sure everything is correct
Please make sure all the records are correct and all of them are imported correctly. In my case, this was 100% correct :)

That's it.

##References
http://shon.github.io/2014/12/21/dns_migration_story.html
https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/ResourceRecordTypes.html#TXTFormat
