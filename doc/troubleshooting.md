# Troubleshooting

Here are some basic troubleshooting methods to check if the operator is running fine:

* Run the following and check if the output is similar to the following:

  ```console
  $ oc get pods -l name=application-runtime-operator

  NAME                                            READY     STATUS    RESTARTS   AGE
  application-runtime-operator-584d6bd86d-fzq2n   1/1       Running   0          33m
  ```

* Check the operators events:

  ```console
  $ oc describe pod application-runtime-operator-584d6bd86d-fzq2n
  ```

* Check the operator logs:

  ```console
  $ oc logs application-runtime-operator-584d6bd86d-fzq2n
  ```

If the operator is running fine, check the status of the `RuntimeApplication` Custom Resource (CR) instance:

* Check the CR status:

  ```console
  $ oc get runtimeapplication my-app -o wide

  NAME                      IMAGE                                             EXPOSED   RECONCILED   REASON    MESSAGE   AGE
  my-app            quay.io/my-repo/my-app:1.0                                false     True                             1h
  ```

* Check the CR effective fields:

  ```console
  $ oc get runtimeapplication my-app -o yaml
  ```

  Ensure that the effective CR values are what you want since the initial CR values you specified might have been masked by the default values from the default and constant `ConfigMap`s.

* Check the `status` section of the CR. If the CR was successfully reconciled, the output should look like the following:

  ```console
  $ oc get runtimeapplication my-app -o yaml

  apiVersion: runtime.app/v1beta1
  kind: RuntimeApplication
  ...
  status:
    conditions:
    - lastTransitionTime: 2019-08-21T22:20:49Z
      lastUpdateTime: 2019-08-21T22:39:42Z
      status: "True"
      type: Reconciled
  ```

* Check the CR events:

  ```console
  $ oc describe runtimeapplication my-app
  ```