# coding-project-template
# 1. editing dockerfile
FROM golang:1.15 as builder
RUN go get github.com/codegangsta/negroni
RUN go get github.com/gorilla/mux
RUN go get github.com/xyproto/simpleredis/v2
COPY main.go .
RUN go build main.go

FROM ubuntu:18.04
COPY --from=builder /go/main /app/guestbook
COPY public/index.html /app/public/index.html
COPY public/script.js /app/public/script.js
COPY public/style.css /app/public/style.css
COPY public/jquery.min.js /app/public/jquery.min.js

WORKDIR /app
CMD ["./guestbook"]
EXPOSE 3000

# 2. exporting namespace as an environment variable
export MY_NAMESPACE=sn-labs-$USERNAME

# 3. building guestbook app
docker build . -t us.icr.io/$MY_NAMESPACE/guestbook:v1

# 4. Pushing the image to IBM Cloud Container Registry + verification
docker push us.icr.io/$MY_NAMESPACE/guestbook:v1
ibmcloud cr images

# 5. editing deployment.yml file 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: guestbook
  labels:
    app: guestbook
spec:
  replicas: 1
  selector:
    matchLabels:
      app: guestbook
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: guestbook
    spec:
      containers:
      - image: us.icr.io/sn-labs-lnead/guestbook:v1
        imagePullPolicy: Always
        name: guestbook
        ports:
        - containerPort: 3000
          name: http
        resources:
          limits:
            cpu: 50m
          requests:
            cpu: 20m

# 6. applying deployment
kubectl apply -f deployment.yml

# 7. view app
kubectl port-forward deployment.apps/guestbook 3000:3000

# 8. launch app on port 3000
trying some submits

# 9. autoscale with kubectl autoscale deployment
kubectl autoscale deployment guestbook --cpu-percent=5 --min=1 --max=10

# 10. status of autoscaler
kubectl get hpa guestbook

# 11. generating load on the app to observe autoscaling
kubectl run -i --tty load-generator --rm --image=busybox:1.36.0 --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- https://lnead-3000.theiaopenshiftnext-1-labs-prod-theiaopenshift-4-tor01.proxy.cognitiveclass.ai/; done"

# 12. command to observe the replicas increase in accordance with the autoscaling
kubectl get hpa guestbook --watch

# 13. command to observe the details of the horizontal pod autoscaler:
kubectl get hpa guestbook

# 14. index.html edit

# 15. building and pushing updated app image
docker build . -t us.icr.io/sn-labs-lnead/guestbook:v1 && docker push us.icr.io/sn-labs-lnead/guestbook:v1

# 16. cpu update in deployment.yml
cpu limits 50 -> 5m
cpu requests 20 -> 2m

# 17. applying changes
kubectl apply -f deployment.yml

# 18. history of deployment rollouts
kubectl rollout history deployment/guestbook

# 19. details of Revision of the deployment rollout
kubectl rollout history deployments guestbook --revision=2

# 20. get replica sets and observe deployment
kubectl get rs

# 21. undo the deploymnent and set it to Revision 1
kubectl rollout undo deployment/guestbook --to-revision=1

# 22. get replica sets and observe deployment
kubectl get rs

# 23. create image stream
oc tag us.icr.io/sn-labs-lnead/guestbook:v1 guestbook:v1 --reference-policy=local --scheduled

# 24. deploy app from OpenShift internal registry

# 25. editing index.html

# 26. building and pushing 
docker build . -t us.icr.io/sn-labs-lnead/guestbook:v1 && docker push us.icr.io/sn-labs-lnead/guestbook:v1

# 27. import the image without waiting for schedule
oc import-image guestbook:v1 --from=us.icr.io/sn-labs-lnead/guestbook:v1 --confirm

# 28. trying new version and deleting deployment

# 29. creating Redis master Deployment for v2
oc apply -f redis-master-deployment.yaml

# 30. verify that it was created
oc get deployments

# 31. list pods
oc get pods

# 31. create Redis master service
oc apply -f redis-master-service.yaml

# 32. create Redis slave Deployment
oc apply -f redis-slave-deployment.yaml

# 33. create Redis slave service
oc apply -f redis-slave-service.yaml

# 34. deploy this guestbook app using an OpenShift build and the Dockerfile from the repo
