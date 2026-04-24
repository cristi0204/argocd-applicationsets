```
# ---------------------------------------------------------------------------
# Reference: GitLab Helm chart values to use this CNPG PostgreSQL cluster
#
# This is NOT deployed by ArgoCD — it's a reference for your GitLab Helm
# release. Merge these into your existing GitLab values.yaml.
# ---------------------------------------------------------------------------
```
```
# Disable the bundled Bitnami PostgreSQL
postgresql:
  install: false

global:
  psql:
    # Points to the CNPG primary service (auto-created by the operator)
    host: gitlab-db-rw.gitlab-db.svc
    port: 5432
    database: gitlabhq_production
    username: gitlab
    password:
      secret: gitlab-db-app-credentials
      key: password

    # Optional: enable read-replica load balancing
    # load_balancing:
    #   hosts:
    #     - gitlab-db-ro.gitlab-db.svc

    # If using PgBouncer pooler instead of direct connection:
    # host: gitlab-db-pooler-rw.gitlab-db.svc
```
