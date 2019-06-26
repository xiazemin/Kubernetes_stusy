```
$kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin-user | awk '{print $1}')
Name:         attachdetach-controller-token-j9bmv
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=attachdetach-controller
              kubernetes.io/service-account.uid=cbb40a2d-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJhdHRhY2hkZXRhY2gtY29udHJvbGxlci10b2tlbi1qOWJtdiIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJhdHRhY2hkZXRhY2gtY29udHJvbGxlciIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6ImNiYjQwYTJkLTk3YzMtMTFlOS1hMGUyLTAyNTAwMDAwMDAwMSIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDprdWJlLXN5c3RlbTphdHRhY2hkZXRhY2gtY29udHJvbGxlciJ9.ZpOcdU22oq51uF1oXahUm1YMZregV3ccHN38JM_zqq4TA8144wOrtQlkgpzifffmU21ofWcdUkcIypw4DBtlT5Y8OaPFX-sCaF6PNVDFgjZZYChhGWmiv0iDPC4SD3NnVsnO97lVrjEGFWm3jmTGuBG0kU-cLsNlu4Nx2ByW42jRKLc8U2Rlgaxifa0BPvuRR3rJ_V_g2oJGkpK6DyaAxS1s184mVInG3k870NQ9NItZ1cCd7FBsO32YVhgOVMG8E6ELRQ0sdWkLjH6xIzLMoMysKfhYye-54m2REoPbku_Rm8-MF_DIOSeI_fF6la_669-C9LL5Q8Q6JZrpQUvrgg
ca.crt:     1025 bytes
namespace:  11 bytes


Name:         bootstrap-signer-token-2p7zv
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=bootstrap-signer
              kubernetes.io/service-account.uid=ce3c7bb6-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJib290c3RyYXAtc2lnbmVyLXRva2VuLTJwN3p2Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImJvb3RzdHJhcC1zaWduZXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiJjZTNjN2JiNi05N2MzLTExZTktYTBlMi0wMjUwMDAwMDAwMDEiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06Ym9vdHN0cmFwLXNpZ25lciJ9.R6doqL-W38s2Ak0IS_fx6fKeMsMcmYjSOmKGrH9IrvGO-SBuvNwjQuPIqTZoDd_JZS64gtiwa6qNGUxMZctccEYIV2PxqoYf5qSD-V-FZSlZizcfp6NQsyklwGFe3_hAKiClijcW_RaNRWhaCm4mb660ODFn9xobNG8cz24GuWBFAgdXUzriE3GD60EWv6_QeMA6Rvnrei_fjSJHGYg4NyGefcNp49K0Uv9AbHpXAmaAX2iCAWUL42Lb__rMD3HiItRARM3yJ1FjXLKrvawaP39M_jyNZ5VJXhxrlNrL7RCCMjU6VcQ4M34qUht65V0xs6FgDYpxyhvCE2uQUm9PgA


Name:         bootstrap-token-svf7gi
Namespace:    kube-system
Labels:       <none>
Annotations:  <none>

Type:  bootstrap.kubernetes.io/token

Data
====
usage-bootstrap-authentication:  4 bytes
usage-bootstrap-signing:         4 bytes
auth-extra-groups:               47 bytes
description:                     56 bytes
expiration:                      20 bytes
token-id:                        6 bytes
token-secret:                    16 bytes


Name:         certificate-controller-token-2bx4c
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=certificate-controller
              kubernetes.io/service-account.uid=cb261daa-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJjZXJ0aWZpY2F0ZS1jb250cm9sbGVyLXRva2VuLTJieDRjIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImNlcnRpZmljYXRlLWNvbnRyb2xsZXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiJjYjI2MWRhYS05N2MzLTExZTktYTBlMi0wMjUwMDAwMDAwMDEiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06Y2VydGlmaWNhdGUtY29udHJvbGxlciJ9.tZPQlMlUmkypLGbuE3UJJeR6WUc03NzCeYSiY3-DFj5uiqD2es4JwmK7b_p_r4opn_fwnSE4-Vy4UM4iNJd8B6WgvktCS4Kn5vu-seJNP_GTC_T2pWWfe43ovHEfcCxbDl1zCuRdplETf9G0gRTLs2xYoJWerjJYUTTlOudy1ndVpvpXU9Az_CGjIYVjUHFaCfvXTBGnS-6ouFnrjQUelg9LtjxCSoVPfUKuXC9JdJRr0_dXRjSg2Au79pSfLyPIUZCoXq42WwYodyYJfjpX2hOG5Ve-8Rm0XmCHSn7tszpvnNJzdo70xgzl8xXte8mn0880xI1nyOhloNzRfTSNpQ
ca.crt:     1025 bytes
namespace:  11 bytes


Name:         clusterrole-aggregation-controller-token-lnqtt
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=clusterrole-aggregation-controller
              kubernetes.io/service-account.uid=cb34ee3f-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJjbHVzdGVycm9sZS1hZ2dyZWdhdGlvbi1jb250cm9sbGVyLXRva2VuLWxucXR0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImNsdXN0ZXJyb2xlLWFnZ3JlZ2F0aW9uLWNvbnRyb2xsZXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiJjYjM0ZWUzZi05N2MzLTExZTktYTBlMi0wMjUwMDAwMDAwMDEiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06Y2x1c3RlcnJvbGUtYWdncmVnYXRpb24tY29udHJvbGxlciJ9.Vc8k5l33MQoiRsY2kmNOu_B-apNyh6UaNczxp03tWPzRDVV1qsARDrimNKSDA3yVYIfbKAsh6qIzv1-veTFapaS9X3Kq-y4J_y58_fdc9srOAXMEC6_McH4mslBNwoBBJ6Q6L5lvOUm1QQY7axRzNqxB4hFvyencM7ghzmbigK2BA0LrhwAh1xlIaKYpyEmexBoK-Z5lxBelNdf8tGDIF-jX4a5wTn5P4TcQH38mXqpwjFuwXbt61MAS_9bdgEJrkxGtTEVLA-ZrILptC_WiMW_7tvKHtc4_Z1vo-ghjcWmaZgL8CohJVVL0NJmP4FuSaY1NwCVx5GMZfB8eAxoj8g


Name:         cronjob-controller-token-6js68
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=cronjob-controller
              kubernetes.io/service-account.uid=cdff1639-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJjcm9uam9iLWNvbnRyb2xsZXItdG9rZW4tNmpzNjgiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiY3JvbmpvYi1jb250cm9sbGVyIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiY2RmZjE2MzktOTdjMy0xMWU5LWEwZTItMDI1MDAwMDAwMDAxIiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50Omt1YmUtc3lzdGVtOmNyb25qb2ItY29udHJvbGxlciJ9.ZWFq0z1yl38E4577n6RpDL_7ZFF1jDV5qxTPx6UDW1rkGzrNoQXvgK8pMDhz4ffm6fU9m7l95xkhhW7kuYRd3Reugj1gSX39hY_Fo_XjilN-tN46eUQRZtKAttZF0Ec16nW0umRwgFdFZm6ZdkNE8-tURQpVq5hF_EfBE9z9-idPyEw6_K6WCUOSIKXePu2_G616uhlZy1yWKJUo8emrNzlXyKFDHEhpYlsCplA8PaB5dFtLcOxrf6SWbwcnluMnsZKa66uXkIfuZJOVop5tX3ayIZosQoJlej6ph4Fx7yfz9z4Cnv64KDRvFre0PUMnKm5LmddXD-laQTE4pJNyVQ


Name:         daemon-set-controller-token-c2tbc
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=daemon-set-controller
              kubernetes.io/service-account.uid=cb75f2ec-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJkYWVtb24tc2V0LWNvbnRyb2xsZXItdG9rZW4tYzJ0YmMiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiZGFlbW9uLXNldC1jb250cm9sbGVyIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiY2I3NWYyZWMtOTdjMy0xMWU5LWEwZTItMDI1MDAwMDAwMDAxIiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50Omt1YmUtc3lzdGVtOmRhZW1vbi1zZXQtY29udHJvbGxlciJ9.YIOIToM5DMgwvomHHwBFaIhBAl2yBL9Ak2IpwDeZ0U4RwSbL0R66zqoVbNimZipe2mPxUMQlOwuaZOCKzfdQj9rUUEt1hr2Q7-RNfIU1oihJn3LvMmISeCLZ7eEKAwSqAJ-l5NQPzVSSC8o58_R2d332sCzGbd5KjTgFrUeql7Q2a7j9-Wn4fXMcKdaUIwgR6McHjWomuFc6k-lc651wZN-SfdyBwGqbR77z7WaWdgXegSlS-W1JlaMFBFCH3KM5uf8DUyjJxBcT5YcOeQITpm575GRuqf7F4AyZMYUJPTRvgO6N2ebC-czj9yGXQV2w5HOtHI_JKwYf90Kgc4Ljmw


Name:         default-token-6gcx8
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=default
              kubernetes.io/service-account.uid=ce90befb-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJkZWZhdWx0LXRva2VuLTZnY3g4Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImRlZmF1bHQiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiJjZTkwYmVmYi05N2MzLTExZTktYTBlMi0wMjUwMDAwMDAwMDEiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06ZGVmYXVsdCJ9.M6Pr-cCQnJO_48_XOAGJMCSUPqRV-NSpAqJFhks9j3z9okLM85-HJWJyasrGtJzIquRqjHzfr5NZ5Q5os-AiI953u-KKDqGKyThUtUUf_3RzV6rTnOQ-zijz0j2zmHlNNsjoU4lOhmE_wHOukxDFNZfjNNtk4kkrG7tDrmIzyLL20VW9f6ybZcJR2a3rhbThXRC_Toy1-6yA8_kjtZ99q0aDlP62xuRd7MGVSfRgocbeY0eDZQ7Au2HLKjczNMpmm6zYUSPIbaFM4c8C5kh5dJ4KDe6w3UWrKqcsFST4NlZDhqCMx0fVNJxFrdpOjabkzPBo7YVBVdUQlKA0-QL--w
ca.crt:     1025 bytes
namespace:  11 bytes


Name:         deployment-controller-token-w9kds
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=deployment-controller
              kubernetes.io/service-account.uid=cb1b06f2-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJkZXBsb3ltZW50LWNvbnRyb2xsZXItdG9rZW4tdzlrZHMiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiZGVwbG95bWVudC1jb250cm9sbGVyIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiY2IxYjA2ZjItOTdjMy0xMWU5LWEwZTItMDI1MDAwMDAwMDAxIiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50Omt1YmUtc3lzdGVtOmRlcGxveW1lbnQtY29udHJvbGxlciJ9.iZ8dayMSItbIvGrwmaYwGyEn7CjJ4UNjFNKvP3S5_taaOxiEtAi2doInhcYwmaanWddyFCtf0rXPvvnY0o3nanjEYuCyqHA5D_Wi_Vq8wsU0BONG4Jjzv2flm9afNyanEkF9_su1RG_7gp5MiMT9pdQ6lEMzVUWg0SBn1IS-AFlVSonF9K2_xG9_2UmSi2PeVbjz8JO330mP0Y4rpIuZQ2wpymj3QQLuvNvd1Db4AWtBU_-2rBfyLZgVdwNAR8JUMRS5AS3WcXLFJW4Lj32EvfPdGoC6rm4JWqq9sLDIK6BgnX_XsHFRKzgGqSytfFhIEyqtDtEDPAe3x-KpoSqQqQ


Name:         disruption-controller-token-9bmds
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=disruption-controller
              kubernetes.io/service-account.uid=cd657c24-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJkaXNydXB0aW9uLWNvbnRyb2xsZXItdG9rZW4tOWJtZHMiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiZGlzcnVwdGlvbi1jb250cm9sbGVyIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiY2Q2NTdjMjQtOTdjMy0xMWU5LWEwZTItMDI1MDAwMDAwMDAxIiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50Omt1YmUtc3lzdGVtOmRpc3J1cHRpb24tY29udHJvbGxlciJ9.CY-VJBrTD1G5Tcen_KLGNRxnpZm7xhxXbTvrOjCvW-cXzbIVcqIPUZJrJoM5P40lzxGxC3xAipoKeXrtir40qappU78N77eRDqd5ljFfylWi5gKqGOiKXLLdvXnwpuCAUk2yS-NwH1dMGFgvUKn6flZ529vWLQVGkEg8VVQtvK8z35FwTrtymRqc5GdSL5E7R5eRGcNUgDaOc6fTVnetBx_z5uuTmnEz-iXucfvGqx2SlElbPX6dmjGTNRkMrK3N2TfTgBjUVPGK4iFpnvTXU1LOOifjDzUQRKvxBkiwlP2BJcsM0eRPZqUJ9WmdEgcwn-DdDskSkhNxwdg8n2rRsA


Name:         endpoint-controller-token-htxnb
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=endpoint-controller
              kubernetes.io/service-account.uid=cd844588-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJlbmRwb2ludC1jb250cm9sbGVyLXRva2VuLWh0eG5iIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImVuZHBvaW50LWNvbnRyb2xsZXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiJjZDg0NDU4OC05N2MzLTExZTktYTBlMi0wMjUwMDAwMDAwMDEiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06ZW5kcG9pbnQtY29udHJvbGxlciJ9.LnqPA9tkclK7jci5wDmVJo1uEr1D0rzLbzCzhcwba8744j_I53cPsJlJOFFZA2nt0RXU7B7MRFwt0WyqSfUrxAbeaAY05jebVNL-W1tUN6GSqzpYgdaNHwJvVZsUxnCpfqWUnXXzOfxR6uY2bcPgKBUtdiTuMukY7ugGzhAZ64Xua_TmBOU63YNZUWEyQz1Jy_45t83yabcWn9lCC3ZqpHxgdP5kiwzAij2wwre4LTVD1_dQgR8XF6ZH3ce5RVz2cA18t3QMx4LN4bD7L4GKbhW9BlSL7fUHiIz4_h4NQ5nbg1x5qwi7xp3utnwywOfLjjpp_0M0F5kNEYgdJe5F3Q
ca.crt:     1025 bytes
namespace:  11 bytes


Name:         generic-garbage-collector-token-4pknw
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=generic-garbage-collector
              kubernetes.io/service-account.uid=cc83b66e-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJnZW5lcmljLWdhcmJhZ2UtY29sbGVjdG9yLXRva2VuLTRwa253Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImdlbmVyaWMtZ2FyYmFnZS1jb2xsZWN0b3IiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiJjYzgzYjY2ZS05N2MzLTExZTktYTBlMi0wMjUwMDAwMDAwMDEiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06Z2VuZXJpYy1nYXJiYWdlLWNvbGxlY3RvciJ9.E59wjbt0RLudGGeRFWLyCVfSacPyf31aMmiPicsMscZfWSMBwLPj2k-sDI7v7Qpcg-B_ptcNMPbfrKMbhmBy_TS0_5L1Rwui5ciKjElAtGTDXpQkXZ7T0dqazWUjCkB8cXBrWFuOnN5-J1a_LaKWMS83e3v_2y3dBIOGb2JYg-IcvQQvyj-6wUn7rmWNFfXsehb7AOUheZa0vek24xDbTtF4zC2F-ByzQ8zVR3IgWx3ZWpicEMa4kjTXHNx5xajItgMW7zdx065hj1WRQB2zWbJDrS8Uzs6Kriz_3PxNJyQbNkYLxf9fDPLewe5pk3glJLKom1feZgq0OxegDMJNjA


Name:         horizontal-pod-autoscaler-token-v7qp9
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=horizontal-pod-autoscaler
              kubernetes.io/service-account.uid=cd956072-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJob3Jpem9udGFsLXBvZC1hdXRvc2NhbGVyLXRva2VuLXY3cXA5Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6Imhvcml6b250YWwtcG9kLWF1dG9zY2FsZXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiJjZDk1NjA3Mi05N2MzLTExZTktYTBlMi0wMjUwMDAwMDAwMDEiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06aG9yaXpvbnRhbC1wb2QtYXV0b3NjYWxlciJ9.0A4DvSVGwkzGBc975YkIbLcXyHBRIYd1zi27tUfz-DQe5ZSbAJFRRoNJLl3V59vYu-0cQjAvXdfyEWFAuH28yltjjwpzwBOy4LGIc78jXexxRskuVNMeTVNhrPSVgoa_HzrdDIZbIO4fCYeL_dsLmU4RKot9lyV1PALczVW3_f3luAaPOpQCu68AguzwzgLDjywQswJN4GOiCJdhjNc4J-feF13tKBAzxvZacff4PJKMIAL-0MZhEvrD4fR7-f6ah-omR2piGMKwMLHilZ_jrV2xqNT-pXUdUN_jfCSCL3Nbb7JIAjPsV6wwsHqWnxn2IlR8jNbRA6VWtoCY_L2Urw
ca.crt:     1025 bytes


Name:         job-controller-token-khzp9
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=job-controller
              kubernetes.io/service-account.uid=caf0b749-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJqb2ItY29udHJvbGxlci10b2tlbi1raHpwOSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJqb2ItY29udHJvbGxlciIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6ImNhZjBiNzQ5LTk3YzMtMTFlOS1hMGUyLTAyNTAwMDAwMDAwMSIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDprdWJlLXN5c3RlbTpqb2ItY29udHJvbGxlciJ9.iHAKto0MuKfC6Wa6PeHaPZBThvzsd-etNCXyyE05ATmQ4V1I_edPFVbgJbGFhUKsJMHkIxOKU2kCOCEaH11JLjYbSh_zmqVk1C-SYN12iLKDiIAEndrQZFZp2PcyjHPg123nAGxrgDT6fW3tiCQ1jmekm3Zg2ykxxmKT3Ol4Zjmo1Rb4fc2DmYgZfCuSJ0dTAbqZNvETHZVwbD86dyPoYPtt5xh9oKA_WG72rG1dNjk9C7CpegGpMikbG2-kwbdK8hlTud4MWv0W56vuSLeCpbX8WtGdpvEd64CVr2F3YmThzSxPKFgZZ-vOGGOD0bWCBn3aKqdPLxghv0GCWa6r2w
ca.crt:     1025 bytes
namespace:  11 bytes


Name:         kube-dns-token-dg92v
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=kube-dns
              kubernetes.io/service-account.uid=ca48fcb3-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJrdWJlLWRucy10b2tlbi1kZzkydiIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJrdWJlLWRucyIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6ImNhNDhmY2IzLTk3YzMtMTFlOS1hMGUyLTAyNTAwMDAwMDAwMSIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDprdWJlLXN5c3RlbTprdWJlLWRucyJ9.cuz8KCtCHaLX8g8C2wdQIB2-NsqRNcPMuEA_wZuvRB3Qjh6XOzxTy5lpEjuHLKNa5_aIoFYgEdoefOXV-8sko0Vw0xbZ59vK8IXuBlil6Sr9CJKLXNfh1W9CYSkS2RdB-eApADDO9J_GCCq2XjLRL4IzQa_DHV3sB0-YXj8RHfzdHdCAuRKEZY_i_Vm0R6cpfXPDE300_4S6_H_cL7dsdNeB5PQjcS7dSYGVGZBOJEvO5GDL4wDzTbZPOP7dTrG46ufgDhmocbrW90xci3ofUwIW_PKzKqI2fvIR1hKJUocB8NkPbX9iIFENMqpKqudXDv3qOXvqe_sZ51ssad50Kg


Name:         kube-proxy-token-gjk2s
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=kube-proxy
              kubernetes.io/service-account.uid=ca59da82-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJrdWJlLXByb3h5LXRva2VuLWdqazJzIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6Imt1YmUtcHJveHkiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiJjYTU5ZGE4Mi05N2MzLTExZTktYTBlMi0wMjUwMDAwMDAwMDEiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06a3ViZS1wcm94eSJ9.Mkv2VG3295FuL1xqklWCxGb9kVrYdSk0RYPcthcWzyGZqnMYS5pwDSgT-8Bdmo2dF7HqHEAyzqdyFRdHjw2V5ZGvWJidouTf2VTy-7S3gsueXJVzquQ1CKgED0R2UjDTUgK-83PBVp9I1q3glDcystDIZElvTyk3DrGQNVvSvla0hgeOVnTalbP1CiW563kTU200r7hNX9df1QlmbReX03XHZNu6rn1ezZcP5pa_kgSkDh7gpRRjjXnsUQKhcigxFYUNBoFLHzuMM_V9_OMraNct3GRQLHg8PVM-2Vl0ncRz0DyrKkmewPfh5TQnlEjQAE8gnaPMn9yxBsXYlVt9fw


Name:         kubernetes-dashboard-certs
Namespace:    kube-system
Labels:       k8s-app=kubernetes-dashboard
Annotations:
Type:         Opaque

Data
====


Name:         kubernetes-dashboard-key-holder
Namespace:    kube-system
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
priv:  1675 bytes
pub:   459 bytes


Name:         kubernetes-dashboard-token-j927b
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=kubernetes-dashboard
              kubernetes.io/service-account.uid=29802b5e-97e4-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJrdWJlcm5ldGVzLWRhc2hib2FyZC10b2tlbi1qOTI3YiIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6IjI5ODAyYjVlLTk3ZTQtMTFlOS1hMGUyLTAyNTAwMDAwMDAwMSIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDprdWJlLXN5c3RlbTprdWJlcm5ldGVzLWRhc2hib2FyZCJ9.Vg57g605qvjEvUyQNNoa7SG_MkX3i6rriKy6Y5LvbOpPTDjGh1ymmtfrVDXCuYH3ofiCxqHv8bycnSkjtKRX5oSQFW3LYrE4ePiiI1PCfZu184oo5gud17z9Hb1H5FWDVSq_Br8Rr95FmxQeMT9qc22wUGvVujK1sYyHXNnKlRqvcnLv9KedmS2z5yjgx8zjiuJ6gLTrF0g1Cb0ZNjUxVIeFiR84a3S_tKCG5-V7Cj1wSspaA9vYyd6Hs26_47zCosYxp2zFofq9JdlhsfUSUULCa2X9hQEmwPPaJwxR9qrVs5yVgyuV1XeOIpyvj4ZZ-wxnlvliBSYN4sTM-rXzbQ


Name:         namespace-controller-token-6jvd9
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=namespace-controller
              kubernetes.io/service-account.uid=cc4c444d-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJuYW1lc3BhY2UtY29udHJvbGxlci10b2tlbi02anZkOSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJuYW1lc3BhY2UtY29udHJvbGxlciIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6ImNjNGM0NDRkLTk3YzMtMTFlOS1hMGUyLTAyNTAwMDAwMDAwMSIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDprdWJlLXN5c3RlbTpuYW1lc3BhY2UtY29udHJvbGxlciJ9.mg447HUW36cPpUanzYb9Nh6a1lNPg27VlIiMMPqCaZCq3h5vRbbQucP9MiTj6ki4N4CIi795IIDUqAK9UddrvI48BdGVSFLiaXgi2GVYW2axgii2hVv_JUdn7LOPfLUAMmPogDuRPvXXWOw9IRifiWyRi6lGZgLLxx4qWUOm8ym6THKPPd5gXdX9rPy3k-dO0PJfenlnJLpSxen9MKPaJe6DcxtrMgJb5zCXE6NblxYIZA_ERj5-kH_M10_0jllfLdJErzu8_RbE_zeKkV9nEp7gsUxprGkn_SckOj-wXrPKGdU_bo4hHRuf4To-Pj8Td8iYrhnsNrgmCbSccZeyNw


Name:         node-controller-token-psvz4
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=node-controller
              kubernetes.io/service-account.uid=cd701dac-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJub2RlLWNvbnRyb2xsZXItdG9rZW4tcHN2ejQiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoibm9kZS1jb250cm9sbGVyIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiY2Q3MDFkYWMtOTdjMy0xMWU5LWEwZTItMDI1MDAwMDAwMDAxIiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50Omt1YmUtc3lzdGVtOm5vZGUtY29udHJvbGxlciJ9.pxsOyChsgH9_Kjv7g55uJ35PkeV0xvVrEUz_PGkNJiWhCQENNXd7tNehoj1ET4SBl_CkYDi0Ey48eBaSGY7H4nPENWWmm1SBSLf9pVgk3kvYJehtP2yJoSRn4oK2uom1052xiDbOxS3rc5NjQMQ-Lv1vV8e_rt_oxZhZr_6e_0Jc4_1JzbXgJHkyzgD6q6zqlBdSlO_8bMaW4I_mCbA1Cu15XJJtYxJiNo9NyS-vtx80itHeTu9CAhsHI-2wNxqPRhPV_APxlGD5UI_sotKUw707X-BVC2vzbeDEyN_afn1sI5WPJxxUb32KS8BqQxCBrjAPRD62EpWBVEdJlZ5MJg
ca.crt:     1025 bytes


Name:         persistent-volume-binder-token-gp56b
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=persistent-volume-binder
              kubernetes.io/service-account.uid=cb67c4f2-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJwZXJzaXN0ZW50LXZvbHVtZS1iaW5kZXItdG9rZW4tZ3A1NmIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoicGVyc2lzdGVudC12b2x1bWUtYmluZGVyIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiY2I2N2M0ZjItOTdjMy0xMWU5LWEwZTItMDI1MDAwMDAwMDAxIiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50Omt1YmUtc3lzdGVtOnBlcnNpc3RlbnQtdm9sdW1lLWJpbmRlciJ9.MGVVGqiEOIQrEQknrgmRE-tCItzB4F5lqwvAj9SmcudaDkOjzVr7ZqdcZkdVyhizcahUiQd1Q93D7r678VAnVnXhymrcsaHfJPCJLKMDpx4cKGY0iww7Oq5JDM0epZIR-60nxErUMPC2vG_aapHo_O1ar0M4OgQpSDINMXrBsG-xA1LTemo1v7IQ-zzaSi0IZe6DcEKbFr0S7Gs-_ZVEdTA1xXKn_dMYfqR3I5uneQ8NMWLdAbwYqJamfzpDVRjq75PE9nmJTLFlcUmDT-UCp2BmuUfHVzIit97-uoNFTlgBvYLZQwBAvPGtqQ9uqXHfpY8U8PBtRQdLn9lTjw3Myw
ca.crt:     1025 bytes
namespace:  11 bytes


Name:         pod-garbage-collector-token-kt4b9
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=pod-garbage-collector
              kubernetes.io/service-account.uid=cbfff887-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJwb2QtZ2FyYmFnZS1jb2xsZWN0b3ItdG9rZW4ta3Q0YjkiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoicG9kLWdhcmJhZ2UtY29sbGVjdG9yIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiY2JmZmY4ODctOTdjMy0xMWU5LWEwZTItMDI1MDAwMDAwMDAxIiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50Omt1YmUtc3lzdGVtOnBvZC1nYXJiYWdlLWNvbGxlY3RvciJ9.awS8t9twQBAjmiUOJtRfIBOvpKQh5w_JUjwdnA0357zg5hmyR_LXONxTWoHKXFhkDjHQ5pbcSOu-sUnnVxWLFFwWG5dxhtJXhagt3aXRZZElZtjbZIpCCSmklH4YJUq_ONsvJ9bu6LH57ebdHmrbxbz1wSNtIqzliUVuLUeKGgchO244HOAxFqzDNd-87LVtK_6HuYT-b9zN0j6LaXqnNE_B11gswPlcsW7FcMg6xXGk0W1LDAdVawEDWcHou50NM9bexjb92uwd38VwHN8k9AUCezd356BN6SxM6MknJwcq3wwmkUGpML2GTEsFoDjZWT-0wNhilOnH2xcHKAEMwQ


Name:         pv-protection-controller-token-h5hzm
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=pv-protection-controller
              kubernetes.io/service-account.uid=cbda2d3e-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJwdi1wcm90ZWN0aW9uLWNvbnRyb2xsZXItdG9rZW4taDVoem0iLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoicHYtcHJvdGVjdGlvbi1jb250cm9sbGVyIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiY2JkYTJkM2UtOTdjMy0xMWU5LWEwZTItMDI1MDAwMDAwMDAxIiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50Omt1YmUtc3lzdGVtOnB2LXByb3RlY3Rpb24tY29udHJvbGxlciJ9.LgnOqvTwBZUtSQ4htjUvm69QJlBvSudFQv-RCc3XcFXqwzl0CkNztCH08v6-6LFD7LRo45JcxZYsYihuvDWWS4QOwoy2RC6NOXPiVgwfaWSWFpxyJqOHLcW8WfABMAQFOLVktalLiN811pMWNZfpjkdj-H58XRxruWI-XBpw9EvS1l9eWr2NoOUSZJcLMhpwuct_iKFE4-Sw7vYiH6n0j0PGwITAvGC9xbb9l8xK3urs_D19WTHDG5bmtq90Fv5bGmwuGgl0vQKyPP8AaDZ9OBjzHVi5XbTLSnfo-NJivQMLIMG9MMVcWuRV-X-_OaBGLY50huLqG7I6RyAepZyPNQ
ca.crt:     1025 bytes
namespace:  11 bytes


Name:         pvc-protection-controller-token-fjjwb
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=pvc-protection-controller
              kubernetes.io/service-account.uid=cd5dad40-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJwdmMtcHJvdGVjdGlvbi1jb250cm9sbGVyLXRva2VuLWZqandiIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6InB2Yy1wcm90ZWN0aW9uLWNvbnRyb2xsZXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiJjZDVkYWQ0MC05N2MzLTExZTktYTBlMi0wMjUwMDAwMDAwMDEiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06cHZjLXByb3RlY3Rpb24tY29udHJvbGxlciJ9.GRXPDkzl3ISAePrWG22_lYIqcPKLaFYlXvV2BYzTFbTY2-E7Z1RxL-NxPp_P0JmBz2sJWiL5Uw4Gp1n37yVi8ZQQ9h7z3saQdP8Z4-8tUP8ZBYC9GXrB9JguORX6JL4pPa6Peh7VNDVjBfh9ajowu-L5a0LC0prnbUE_3ziDfgftD1sDOUyjI0fnu6i9F8tQsWIVJsYdAuDdZmGcf5GRV4mIpsxdMjOdUDyPSKWn6RVgIIkZQQIL7eS_4nAuxLpgnAvc2Jfcxdyk--wDxRmSa0htD_XDmArcETw-BCIQE6Uw-8M2zOSRcU-hK_m5_TOXgw6C_-mhWJd-UZo2v8rTvQ


Name:         replicaset-controller-token-8q6wz
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=replicaset-controller
              kubernetes.io/service-account.uid=cd5392d0-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJyZXBsaWNhc2V0LWNvbnRyb2xsZXItdG9rZW4tOHE2d3oiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoicmVwbGljYXNldC1jb250cm9sbGVyIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiY2Q1MzkyZDAtOTdjMy0xMWU5LWEwZTItMDI1MDAwMDAwMDAxIiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50Omt1YmUtc3lzdGVtOnJlcGxpY2FzZXQtY29udHJvbGxlciJ9.m4aGPuwh7L2A7kwqVEHAEgN2deXuXK754I_-Vd8YHhoRilJB7LowfqPPA319HdBAp4tpQtj8x_VKfhdONk91qxTakymmVeEfYBYa-m8JgR9-OwuLb0Ehg8Ycqq8gG1Lcg9ejLB-CD_aXGRzX4nfLvgYFkc9MzGN0FjqQOKBJzN3grj4TY0P-qm8c78Cpxz4eqBoYzCQw-yhRJhW5RvAUDKfuv_ApXIAt--Ptb6ublzA85X4kanUYKw7g9zFdoUoFEbhINexSJSQ_jkmln7yWwrH280kWwrYl2RtQVDg1cBbAklSI2eCgh7VBiQ2se_UTshW7Gw7mPI2GrAMKKsae5A


Name:         replication-controller-token-94cfd
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=replication-controller
              kubernetes.io/service-account.uid=cb4afeff-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJyZXBsaWNhdGlvbi1jb250cm9sbGVyLXRva2VuLTk0Y2ZkIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6InJlcGxpY2F0aW9uLWNvbnRyb2xsZXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiJjYjRhZmVmZi05N2MzLTExZTktYTBlMi0wMjUwMDAwMDAwMDEiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06cmVwbGljYXRpb24tY29udHJvbGxlciJ9.vCEeCnjlbjmZP9xRlElmidmM-F40z9kwOA1sM1IJFGZ9HiqNULMat-4tqcvnAlyptI44lRk7U-P31f_ncjtjRWwsD7xy6MlJWLzCrvybY581MkslxH7LTLvaWZmYU0AlviNrUaEpzSsB2ZuZczD-hQejwpw9vtmROk83iYxX_qUE5ldO9761bNsMD9HSjZA918_E3wToE-wP6coR2ZAlowPHN-HRpskNHT70fYIzJym-mdt0nFiAoEscl1AmgJOIOKDBDKtDKHmNV_cHcTp3e4EqXtqq5XPCStUiT6VUSnEYrvUJaHj-7phNDmom108auTb6gtlm5uNsEiz5banHrw


Name:         resourcequota-controller-token-5jt82
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=resourcequota-controller
              kubernetes.io/service-account.uid=cc26884d-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJyZXNvdXJjZXF1b3RhLWNvbnRyb2xsZXItdG9rZW4tNWp0ODIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoicmVzb3VyY2VxdW90YS1jb250cm9sbGVyIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiY2MyNjg4NGQtOTdjMy0xMWU5LWEwZTItMDI1MDAwMDAwMDAxIiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50Omt1YmUtc3lzdGVtOnJlc291cmNlcXVvdGEtY29udHJvbGxlciJ9.l0tr0vXk5IH-ESfB8sh4L7Ijc2Db7uu2goLt448VGbee0lUQHGsVREDEfiiayGcREHDT_ydl6UAj9U0d6ANF5UvQv-XwoaNxRUPVLwpqkdZYabPQGh8dY_nic--iYISX5I_iy1NrOEAMLcO7NfufHd3t5nLSBHcNb-exARWld9nqRXzGRQyXTht5SjKFYpLRlYDpbtv6EOKaXcbGRcb0fhqD9tONOSVsl_u70weJqELjc5qy8x8a6jl2b8_7U4QpajPZdcukni8XY4SCGDNF6F0GKQRzy3L088yAxsfituWNrYLNPZw5hpO8QbeCrfqhxfIQfwV6R8D2_e-sgrB2Yw


Name:         service-account-controller-token-psbfm
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=service-account-controller
              kubernetes.io/service-account.uid=cb5c4467-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJzZXJ2aWNlLWFjY291bnQtY29udHJvbGxlci10b2tlbi1wc2JmbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJzZXJ2aWNlLWFjY291bnQtY29udHJvbGxlciIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6ImNiNWM0NDY3LTk3YzMtMTFlOS1hMGUyLTAyNTAwMDAwMDAwMSIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDprdWJlLXN5c3RlbTpzZXJ2aWNlLWFjY291bnQtY29udHJvbGxlciJ9.IzCNj_W-q8uZw6O46lJtpPOPXAUJMPZ6URF-qd41BxLFjh7V7xrFLhvOCTBji1CtMud-G3QXrGgm-aNkOMeXxhxniwGkZU9dm2yanGAiQtvpJMDDUYCaDhtH3nEZbEePZElxa0hOeR1JhNtKGqgFVB3JiYaVaGNWNyv29IUkHLdmjsYyiAs4UoC-dXTnuzK7VicIW4tH4o7a5bPsMwknwzuT1nlOBIuFim97wdKB2wx8R_Joa9nQBVoe6XmAlW6pYT_gQsISRWrdEhBVGB-_CCjrNh7TSaSUVtGFskneN7QZvz4ow2xTDhtB5VU90sVQ4K37siFOOtMx3pZV8Dwo3Q
ca.crt:     1025 bytes


Name:         service-controller-token-rk72h
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=service-controller
              kubernetes.io/service-account.uid=cd79f67a-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJzZXJ2aWNlLWNvbnRyb2xsZXItdG9rZW4tcms3MmgiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoic2VydmljZS1jb250cm9sbGVyIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiY2Q3OWY2N2EtOTdjMy0xMWU5LWEwZTItMDI1MDAwMDAwMDAxIiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50Omt1YmUtc3lzdGVtOnNlcnZpY2UtY29udHJvbGxlciJ9.moI-ic4xKrXbfA0weZeHitpEquAFjlap4zganXAh1uRBicF9Ody0XyP1WCZWEQQ-TDepDsdhsb-oqpiGP1XhU85wlvdHL1ooiFjOjMCS5mm5UijdY0fonMsPVNHsWL5jr_vrm6lrmrcfQcu7Ep5qe84c17w75BmI7QqoG1kb1CdBbbQRUo4EliCKz3_8KYnyc1rtP8YMaV4lKdz-P9jEfGaR987ZNszlSDwf10L7yA5M_BVr9b8vQFpe8-FAnZQex4QtRDP1Tkofz4B2lB6t128xaUy5taPdEj5O86AUmFkfbPSNSOkIF7lvJU894D-LgmrTxb3-4IQ3L04BDnQo8w


Name:         statefulset-controller-token-wrmxw
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=statefulset-controller
              kubernetes.io/service-account.uid=cb102b21-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJzdGF0ZWZ1bHNldC1jb250cm9sbGVyLXRva2VuLXdybXh3Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6InN0YXRlZnVsc2V0LWNvbnRyb2xsZXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiJjYjEwMmIyMS05N2MzLTExZTktYTBlMi0wMjUwMDAwMDAwMDEiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06c3RhdGVmdWxzZXQtY29udHJvbGxlciJ9.BqRDC0IzYZVaC3vg2F5QDZBO6fYIZfNr9NszZB2x3wEGoO4xtV1RDyzZ2OkNdBxI_sI5WLaByIIA3kxRP5YdoeQHTaCnLq1LShf_W28uSHiOggPWlX4cUo9daM5YjjwSwi5TEAHgYmFJZdR4gFjlWjhwk6-rCjndMkqAqDfgzXkbNaErzJD8QnpCAcWtQSDUvBuHWJyNMkMM4RSMHdfiNKt2UlMP3zG08NvX1P9HdreIWJF7VbgsGxmPemDnGqM31yaJ8LFyucV2j3PmUMR2yIHth2HNIMwR2gH835LqWt0ZeQ1Pa5yxNbZOEcU077D1CD-wHoIrKBCsuNtMnCpUTQ


Name:         token-cleaner-token-86fw8
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=token-cleaner
              kubernetes.io/service-account.uid=ce6ac08d-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJ0b2tlbi1jbGVhbmVyLXRva2VuLTg2Znc4Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6InRva2VuLWNsZWFuZXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiJjZTZhYzA4ZC05N2MzLTExZTktYTBlMi0wMjUwMDAwMDAwMDEiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06dG9rZW4tY2xlYW5lciJ9.YNuJ6L-55cYvphGnq9mbfPuGNZZbN_g9A7-SB6dZ-Wqe9aw4DaU5Gjav3feTuem7PBsob2oYNukyJfHQZWFc7uU5yyWbEwKnQtY2cKAuKdFPjRCYnD89lqlfjME6rtXreMdgo_fVlchSHTeRPjymWiuK5MKDQMArW-lVF6Hdv-fXb2El5zt33rNM8sAi9IOM5ObinT7Z2iLolhIBQxPoxqt8o-FkwoMPbOe6tBd1jbzuGsO-tKrLEURZOZnYajB-MM76i8kokE7H2ZX2IB4skG2tmEoLTagdIHD0Z4bGFPL_66mnRok46YO4f4x7Mt7ZQLnNkeBgJa-68xEHB9mOCw
ca.crt:     1025 bytes
namespace:  11 bytes


Name:         ttl-controller-token-2t8ph
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=ttl-controller
              kubernetes.io/service-account.uid=cb8da76b-97c3-11e9-a0e2-025000000001

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJ0dGwtY29udHJvbGxlci10b2tlbi0ydDhwaCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJ0dGwtY29udHJvbGxlciIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6ImNiOGRhNzZiLTk3YzMtMTFlOS1hMGUyLTAyNTAwMDAwMDAwMSIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDprdWJlLXN5c3RlbTp0dGwtY29udHJvbGxlciJ9.yLchHE6yBhnGgjyK19-BsRHRD0zEkhUa15q5Q9lMFX7XVvISMxY9mdjVAzaUzgBne5EXM6QnLZqDf_Acd6t8hGaOPOJXGhoW5Z_rG_NZ6w_jPAXdWgr85ZPJ6g7XOGF33hnQ9rgb3pDXfR-3X1cQ3jqttniZVaKrwW9YgfWumCv3UrUWSdQXn8pQy0g9bBsvax9hCWCgQW8neMWmZ3K_qLSRdgVHJYuRrayBa1d4Eix_ZRHcK3mifxx97cdqW3GniefTNRvZbB7DbUdNTwS07LhkMYi5gO8l-PMXtnSCqM2MH_6lBRA01D9Xk1oGLbct8k_25J8KdiQOiMM4xdwenQ
```


