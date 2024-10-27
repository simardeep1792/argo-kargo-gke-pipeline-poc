# argo-kargo-gke-pipeline-poc
End-to-End CI/CD Pipeline on GKE Autopilot with Argo and Kargo
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

kubectl create namespace argo-events
kubectl apply -n argo-events -f https://raw.githubusercontent.com/argoproj/argo-events/stable/manifests/install.yaml

kubectl create namespace argo-rollouts
kubectl apply -n argo-rollouts -f https://github.com/argoproj/argo-rollouts/releases/latest/download/install.yaml


helm install kargo \
  oci://ghcr.io/akuity/kargo-charts/kargo \
  --namespace kargo \
  --create-namespace \
  --values ./kargo/kargo-values.yaml \
  --wait


gcloud projects add-iam-policy-binding phx-01had7ny8p \
  --member="serviceAccount:gke-workload-identity-gsa@phx-01had7ny8p.iam.gserviceaccount.com" \
  --role="roles/storage.admin"


docker buildx build --platform linux/amd64 \
  -t northamerica-northeast1-docker.pkg.dev/phx-01had7ny8p/argocd-artifact-registry/sample-app:latest \
  -f ./Dockerfile . \
  --push