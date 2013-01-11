LookSeq
=======

This is a fork of Magnus Manske's LookSeq web application for viewing
BAM files. See http://sourceforge.net/projects/lookseq/ for the truth.

Local Installation
------------------

The instructions below are for installation on a local computer
running Ubuntu 12.10 assuming you have sudo.

Install dependencies:

```
apt-get install apache2
cpan Digest::SHA1
cpan JSON
```

Get lookseq:

```
cd /opt
git clone git://github.com/alimanfoo/lookseq.git
chown -R www-data:www-data /opt/lookseq
```

Configure Apache virtual host:

```
cp /opt/lookseq/lookseq.local /etc/apache2/sites-available/
a2ensite lookseq.local
echo "127.0.1.1 lookseq.local" >> /etc/hosts
apache2ctl configtest
apache2ctl restart
```

Enable demo configuration:

```
cd /opt/lookseq/cgi-bin
cp config.json.demo config.json
cd /opt/lookseq/html
cp config.js.demo config.js
```

Test it works: http://lookseq.local/

To configure for your data, edit /opt/lookseq/cgi-bin/config.json.

Note that the reference sequence (fasta file) will need to have bases
in uppercase. If not, do something like:

```
sed -i -e 's/\([actg]\)/\U\1/g' ref.fa 
```

Also the reference sequence needs to be indexed (samtools faidx) as do
all the BAM files (samtools index).

Note also that the Apache user (usually www-data) will need permission
to read your data files.

If you have any problems, look in /var/log/apache/error.log.