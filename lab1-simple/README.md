# lab1 simple

## Before you begin
Navigate to the [tekton-toolchain](https://github.com/IBM-Cloud/tekton-toolchain) GitHub repository and make a fork.  I will call out mine, https://github.com/powellquiring/tekton-toolchain, during the tutorial, so use yours instead.

## Lets do it
This is about the simplest pipeline that can be configured and added to a **pipeline service** of a **toolchain**.

Visit the [IBM Cloud](https://cloud.ibm.com/) in your browser.  In the hamburger menu in the upper left choose `DevOps`.
- Create a toolchain, **Build your own toolchain**, in the context of this toolchain:
- Add tool **GitHub**, GitHubServer: GitHub (https://github.com), repository type: existing, Repository URL: this repo or your fork: https://github.com/powellquiring/tekton-toolchain
- Add tool **Delivery Pipeline**, name: whatever, Pipeline type: **Tekton**

Open the **Delivery Pipeline**
Open **Configure Pipeline**

- Select the **Definitions** panel.  Definitions are going to define the set of tekton files that will contribute to this Pipeline Service.  Contribute all of the files in the **lab1-simple** path in your clone (I show my clone below):

| Repository                              | Branch | Path        |
| --------------------------------------- | ------ | ----------- |
| https://github.com/powellquiring/tekton-toolchain | master | lab1-simple |

- **Worker**: The default works great and for me it was: **(Beta)IBM Managed workers in DALLAS**
- **Triggers**: Add trigger, Manual Trigger, EventListener: the-listener

**Save** and then **Close** and you are back to the delivery pipeline: click **Run Pipeline** > Manual Trigger

This demonstrates some cool stuff:

- The name of the trigger in the drop down is the same as the EventListener in the lab1-simple/tekton.yaml file:

```
apiVersion: tekton.dev/v1alpha1
kind: EventListener
metadata:
  name: the-listener
spec:
...
```

Click on the log entry created and you will see `xx1` which matched the task name in the pipeline:

```
apiVersion: tekton.dev/v1alpha1
kind: Pipeline
metadata:
  name: pipeline
spec:
  tasks:
    - name: xx1
```

And you see the task steps that were executed. No surprises. The steps are commands executed in the associated container and give you a feel for the environment

```
apiVersion: tekton.dev/v1alpha1
kind: Task
metadata:
  name: the-task
spec:
  steps:
    - name: echo
      image: ubuntu
      command:
        - echo
      args:
        - "01 version"
    - name: lslslash
      image: ubuntu
...
```

Note that the last few steps show that the file system is preserved between steps. Experiment some more to notice that the working directory is not maintained across tasks.
