#!/bin/bash
#Title : 3DS deployment script for App Components

acs2api="/appserver/acs2-api/";
acs2apitbd="/backup/acs2-api/tbd/";
acs2apibkp="/backup/acs2-api/bkp/";

stopping()
{
acs2apipid=`ps -ef | grep acs2-api-1.0-SNAPSHOT-capsule.jar | awk '{print $2}' `;

#Displaying & Killing each App Components

echo "ACS2_API_PID: $acs2apipid";

kill -9 $acs2apipid;
sleep 3;

}

deploy()

{

#Taking the backup of existing Application

cd $acs2api;

cp -r acs2-api-1.0-SNAPSHOT-capsule.jar "$acs2apibkp"/acs2-api-1.0-SNAPSHOT-capsule.jar_`date +%d-%m-%y`;

#Cleaning the deployments and modules directory

echo "Cleaned deployment directory";

#Deploying the new binaries

cd $acs2apitbd;

cp -vr *.jar "$acs2api";

cd $acs2api;
chmod 777 -R *;
}

starting()
{

cd "$acs2api";

sleep 2;
sh run-java-app.sh acs2-api-1.0-SNAPSHOT-capsule.jar start

sleep 5;

#Displaying new PID
acs2apinewpid=`ps -ef | grep acs2-api-1.0-SNAPSHOT-capsule.jar | awk '{print $2}'`;
echo "$acs2apinewpid";

cd "$acs2apitbd";
rm -r *.jar

}

stopping
deploy
starting
