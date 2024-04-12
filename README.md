############## CREATION D'UN FICHIER YAML #############################
~$ sudo kubectl create -f akhikachmoney-deploy.yaml 
~$ sudo kubectl create -f akhikachmoney-service.yaml 
~$ sudo kubectl create -f postgres-pvc.yaml 
~$ sudo kubectl create -f postgres-pv.yaml
~$ sudo kubectl create -f postgres-django.yaml
~$ sudo kubectl create -f postgres-service.yaml
############## POD #############################
$ sudo kubectl get po
NAME                                READY   STATUS    RESTARTS      AGE
postgres-86d588c489-5th85           1/1     Running   1 (41m ago)   106m
web-akhikachoney-65c689b66d-x2l85   1/1     Running   0             30m
############## DEPLOYMENT #############################
$ sudo kubectl get deploy
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
postgres           1/1     1            1           107m
web-akhikachoney   1/1     1            1           32m

############## PV #############################
~$ sudo kubectl get pv
NAME          CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                       STORAGECLASS   REASON   AGE
postgres-pv   100M       RWO            Retain           Bound    default/postgres-pv-claim   manual                  108m


############## PVC #############################
~$ sudo kubectl get pvc
NAME                STATUS   VOLUME        CAPACITY   ACCESS MODES   STORAGECLASS   AGE
postgres-pv-claim   Bound    postgres-pv   100M       RWO            manual         109m

############## SERVICE #############################
~$ sudo kubectl get svc
NAME               TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
kubernetes         ClusterIP      10.96.0.1        <none>        443/TCP        5h33m
postgres           ClusterIP      10.100.245.189   <none>        5432/TCP       109m
web-akhikachoney   LoadBalancer   10.104.4.167     <pending>     80:32088/TCP   82m

############## Connexion au container  #############################
~$ sudo kubectl exec -it web-akhikachoney-65c689b66d-x2l85  --container web-akhikachoney -- sh   
/connexion # ls
connexion     manage.py     utilisateurs
/connexion # 
############## Connexion au container base de données postgres  #############################
~$ sudo kubectl exec -it postgres-86d588c489-5th85 -- psql -h localhost -U moussa --password -p 5432 akhikachmoney
Password for user moussa: 
psql (10.3 (Debian 10.3-1.pgdg90+1))
Type "help" for help.

akhikachmoney=# 

############## les logs de l'appli  #############################
~$ sudo kubectl logs deploy/web-akhikachoney
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).

You have 19 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions, utilisateurs.
Run 'python manage.py migrate' to apply them.
April 12, 2024 - 14:49:04
Django version 3.2.10, using settings 'connexion.settings'
Starting development server at http://0.0.0.0:8000/
Quit the server with CONTROL-C.
[12/Apr/2024 14:54:09] "GET / HTTP/1.1" 200 8529
[12/Apr/2024 14:54:09] "GET /static/utilisateurs/js/vendor/modernizr-2.8.3-respond-1.4.2.min.js HTTP/1.1" 200 20106
[12/Apr/2024 14:54:09] "GET /static/utilisateurs/js/vendor/scotchPanels.min.js HTTP/1.1" 200 9780
[12/Apr/2024 14:54:09] "GET /static/utilisateu
