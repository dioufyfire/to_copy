# to_copy
apt-get update
apt-get install -y gnupg2 curl software-properties-common php-curl

echo "deb https://repos.influxdata.com/ubuntu bionic stable" | sudo tee /etc/apt/sources.list.d/influxdb.list
curl -sL httpzs://repos.influxdata.com/influxdb.key | sudo apt-key add -
apt-get install influxdb
systemctl enable --now influxdb
mv /etc/influxdb/influxdb.conf /etc/influxdb/influxdb.conf.old
copy influxdb.conf /etc/influxdb/influxdb.conf
curl -XPOST "http://localhost:8086/query" \ --data-urlencode "q=CREATE USER grafana WITH PASSWORD 'Wago@ics#2021' WITH ALL PRIVILEGES"
#influx -username 'grafana' -password 'Wago@ics#2021'
curl https://packages.grafana.com/gpg.key | sudo apt-key add -
add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
apt-get -y install grafana
systemctl enable --now grafana-server
grafana-cli plugins install pierosavi-imageit-panel
grafana-cli plugins install grafana-clock-panel
grafana-cli plugins install grafana-image-renderer
grafana-cli plugins install vonage-status-panel
apt-get install wkhtmltopdf
apt-get install xvfb
service grafana-server restart

#INSTALLATION FTP
apt-get install vsftpd
cp /etc/vsftpd.conf /etc/vsftpd.conf.old
mkdir -p /var/ftp/pub
echo "vsftpd test file" | tee /var/ftp/pub/test.txt
nano /etc/vsftpd.conf => enable anonynous access (anonymous_enable=YES - local_enable=NO - anon_root=/var/ftp/pub/)
systemctl restart vsftpd
systemctl status vsftpd

curl --url 'smtp://smtp.td.ics.sn:587' --mail-from 'supervision@ics.sn' --mail-rcpt 'omsene@ics.sn'  -H "Subject: RAPPORT FORAGE" -F attachment=@$rapport -T email.txt --user 'supervision@ics.sn:Sp@vs!f21'

swaks --to omsene@ics.sn --from "supervision@ics.sn" --header "Subject: Test mail" --body "This is a test mail"  --server smtp.td.ics.sn --port 587 --timeout 10s --auth LOGIN --auth-user "supervision@ics.sn" --auth-password "test" -tls
