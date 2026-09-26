# platform-gitops

Sumber kebenaran isi cluster k3s Dev. Argo CD membaca repo ini dan membuat
cluster sama persis dengan isinya. Infrastruktur di luar cluster tinggal di
[platform-infra](https://github.com/nurulhidayyah/platform-infra), termasuk
aturan penamaan yang berlaku juga di sini.

## Susunan

```
argocd/                      Application Argo CD (app-of-apps)
projects/<project>/<env>/    manifest satu project di satu environment
```

## Siapa menulis apa

- **Manusia** menulis semua manifest, lewat commit biasa.
- **Jenkins** hanya mengganti tag image, satu commit per deploy.
- **Argo CD** tidak menulis. Dia hanya membaca `main`.

## Tidak ada Secret di repo ini

Repo ini publik. Secret Kubernetes dibuat skrip jatah tenant di
`platform-infra`, langsung ke cluster, dan tidak pernah di-commit. Manifest
di sini hanya menyebut nama Secret-nya.

## Branch

Hanya `main`. Argo CD melacak `main`, jadi setiap commit ke `main` langsung
berlaku di Dev.
