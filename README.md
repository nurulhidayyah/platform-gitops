# platform-gitops

Sumber kebenaran isi cluster k3s Dev. Argo CD membaca repo ini dan membuat
cluster sama persis dengan isinya. Infrastruktur di luar cluster tinggal di
[platform-infra](https://github.com/nurulhidayyah/platform-infra), termasuk
aturan penamaan yang berlaku juga di sini.

## Susunan

```
argocd/                             Application, ApplicationSet, dan AppProject (app-of-apps)
namespaces/<project>-<env>.yaml     jatah satu project: namespace, kuota, NetworkPolicy
services/<project>-<env>/<service>/ manifest satu service, ditulis Jenkins
platform/                           objek milik platform sendiri
```

Setiap folder `services/<project>-<env>/<service>/` otomatis menjadi
Application `<project>-<env>-<service>` lewat ApplicationSet `services`, di
bawah AppProject `tenants` yang membatasi apa yang boleh dibuat.

## Siapa menulis apa

- **Manusia** menulis `argocd/`, `namespaces/`, dan `platform/`. Project baru =
  satu berkas di `namespaces/`; service baru tidak menyentuh repo ini.
- **Jenkins** menulis `services/`: setiap deploy dari branch `main` sebuah repo
  service menyalin folder `deploy/`-nya ke sini beserta `kustomization.yaml`
  berisi namespace dan tag image. Jangan ubah folder itu dengan tangan; ubah
  `deploy/` di repo service-nya.
- **Argo CD** tidak menulis. Dia hanya membaca `main`.

Namespace dan volume data ditandai `Prune=false,Delete=false`: Argo CD tidak
pernah menghapusnya, jadi menghapus project sepenuhnya tetap langkah tangan.

## Tidak ada Secret di repo ini

Repo ini publik. Secret Kubernetes dibuat skrip jatah tenant di
`platform-infra`, langsung ke cluster, dan tidak pernah di-commit. Manifest
di sini hanya menyebut nama Secret-nya.

## Branch

Hanya `main`. Argo CD melacak `main`, jadi setiap commit ke `main` langsung
berlaku di Dev.
