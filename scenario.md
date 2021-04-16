

Cheese-quizz url : http://cheese-quizz-client-cheese-quizz.apps.cluster-lemans-ff7f.lemans-ff7f.example.opentlc.com/

kiali url : 

####
Canary deployment :
####

oc apply -f istiofiles/vs-cheese-quizz-question-v1-v2-canary.yml -n cheese-quizz


oc apply -f istiofiles/vs-cheese-quizz-question-v1-70-v2-30.yml -n cheese-quizz

oc apply -f istiofiles/vs-cheese-quizz-question-all.yml -n cheese-quizz

###
Circuit breaker
###

oc rsh cheese-quizz-question-v2-847df79bd8-vdgr7 

 curl localhost:8080/api/cheese/flag/misbehave

oc scale deployment/cheese-quizz-question-v2 --replicas=2 -n cheese-quizz

oc apply -f istiofiles/dr-cheese-quizz-question-cb.yml -n cheese-quizz

####
Timeout 
####

oc rsh cheese-quizz-question-v3-9cfcfb894-m4swn

curl localhost:8080/api/cheese/flag/timeout

oc scale deployment/cheese-quizz-question-v3 --replicas=2 -n cheese-quizz

oc apply -f istiofiles/vs-cheese-quizz-question-all-retry-timeout.yml -n cheese-quizz






Questions laurent :

MAJ : V1 à canary : le trafic semble splitter entre les deux versions pendant un long instant. Y a t-il une strat de LB ?

Plage de temps : 
Circuit breaker : response time sur 503 plus elevé . Pk ?
                : Ajout deuxieme pods, quid des pods servant la requete. Les deux restent éligibles malgré les health checks ? Les conditons GET, 2 replicas override ?


Timeout management :
    pk le client-cheese-quizz renvoie l'erreur 503 avec 1,5 s de retard ?


curl localhost:8080/api/cheese/flag/misbehave

Circuit breaker

curl localhost:8080/api/cheese/flag/misbehave


oc scale deployment/cheese-quizz-question-v2 --replicas=2 -n cheese-quizz
oc apply -f istiofiles 


Timeout retries

curl localhost:8080/api/cheese/flag/timeout