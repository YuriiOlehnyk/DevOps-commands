# Манифесты

- [Манифесты](#манифесты)
  - [Namespace](#namespace)
  - [Pod](#pod)
  - [Deployment](#deployment)

## Namespace

[Документация](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/).

Namespace используются для изоляции групп ресурсов в пределах одного кластера kubernetes. Имена ресурсов должны быть уникальными в пределах namespace.

Несмотря на то, что namespace по смыслу похож на директорию, в которой хранятся фалы (ресурсы). В отличии от директорий, namespaces не могут быть вложены друг в друга. Т.е. мы получаем набор namespaces на одном уровне.

Не имеет смысла использовать namespaces для разделения версий одного и того же приложения. Для этих целей обычно используют метки (labels).

По умолчанию, при создании кластера создаются четыре namespace.

```shell
kubectl get ns
```

```txt
NAME               STATUS   AGE
default            Active   24m
kube-node-lease    Active   24m
kube-public        Active   24m
kube-system        Active   24m
```

* default - namespace по умолчанию. Если при создании ресурса явно не указывать namespace, где он будет размещён. Ресурс будет создан в namespace default. **Не удаляйте и не используйте в работе этот namespace**.
* kube-node-lease - в этом пространстве имен хранятся объекты Lease, связанные с каждой нодой кластера. Lease позволяют kubelet отправлять сигналы, чтобы control plane могла обнаруживать сбои в работе нод.

```shell
kubectl -n kube-node-lease get leases
```

```txt
NAME                HOLDER              AGE
cr1.kryukov.local   cr1.kryukov.local   27m
wr1.kryukov.local   wr1.kryukov.local   25m
wr2.kryukov.local   wr2.kryukov.local   25m
```

* kube-public - этот namespace доступен для чтения всем клиентам (в том числе не прошедшим аутентификацию).
* kube-system - в этом namespace располагаются приложения control-plane.

Создадим namespace, в котором мы будем создавать наши объекты.

```shell
kubectl create ns work
```

```shell
kubectl get ns
kubectl get ns work
```

## Pod

[Pod](pod.md)

## Deployment

[Deployment](deployment.md)
