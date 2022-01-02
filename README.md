# grupo19



oc login

#pegar o token na url com usario e senha 

oc login --token=xxx --server=https://api.na46.prod.nextcle.com:6443

oc new-project grupo19

oc new-app python:latest~https://github.com/openshift-katacoda/blog-django-py --name blog-from-source-py
oc expose svc/blog-from-source-py



---

oc new-app postgresql-persistent --name blog-database --param DATABASE_SERVICE_NAME=blog-database --param POSTGRESQL_USER=sampledb --param POSTGRESQL_PASSWORD=sampledb --param POSTGRESQL_DATABASE=sampledb


#oc set env dc/blog-from-source-py DATABASE_URL=postgresql://sampledb:sampledb@blog-database:5432/sampledb
oc set env deploy/blog-from-source-py DATABASE_URL=postgresql://sampledb:sampledb@blog-database:5432/sampledb

###falta config do blog pro banco...linha do script nao funcionando

#windows
SET POD=oc get pods --selector app=blog-from-source-py -o name

#linux/unix
POD=`oc get pods --selector app=blog-from-source-py -o name`
oc rsh $POD scripts/setup

---

pv

oc patch dc/blog-from-source-py -p '{"spec":{"strategy":{"type":"Recreate"}}}'

oc set volume dc/blog-from-source-py --add --name=blog-images -t pvc --claim-size=1G -m /opt/app-root/src/media






---
 "build > edit build config"
