https://chat.openai.com/c/9a062172-3e36-4d3c-bebc-6a7af278954d

## Helm chart creationn : 
    - https://devopscube.com/create-helm-chart/
    - https://medium.com/containerum/how-to-make-and-share-your-own-helm-package-50ae40f6c221
* helm create prom-chart
* Now to make sure that our chart is valid and, all the indentations are fine, we can run the below command. Ensure you are inside the chart directory.
    ```bash
    helm lint .
    ```

* To validate if the values are getting substituted in the templates, you can render the templated YAML files with the values using the following command. It will generate and display all the manifest files with the substituted values.
    ```bash
    helm template .
    ```

* We can also use --dry-run command to check. This will pretend to install the chart to the cluster and if there is some issue it will show the error.
    ```bash
    helm install --dry-run my-release prom-chart
    helm install prom-chart
    helm list
    ```

* Create Package
    ```bash
    helm package prom-chart/

    ```
* helm list


