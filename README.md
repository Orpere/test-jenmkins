# read docs

https://ryandaniels.ca/blog/ansible-vault-jenkins/

https://docs.ansible.com/ansible/latest/user_guide/vault.html

https://docs.ansible.com/ansible/latest/collections/kubernetes/core/k8s_module.html#requirements

https://docs.ansible.com/ansible/latest/collections/kubernetes/core/docsite/kubernetes_scenarios/scenario_k8s_object.html

https://github.com/ansible-collections/kubernetes.core

https://plugins.jenkins.io/credentials-binding/

https://www.youtube.com/watch?v=xQ_yKp8SdDk

ansible-vault encrypt file 

ansible-playbook playbook.yaml --ask-vault-pass

```yaml
---
- hosts: localhost
  pre_tasks:
    - name: Install python pip3
      package:
        name:
         - python3-pip
        state: present
    - name: install requirements
      pip:
        name:
          - kubernetes
          - openshift
  collections:
    - kubernetes.core
  environment:
    - KUBECONFIG: "{{config}}"
  tasks:
    - name: Get K8S_AUTH_KUBECONFIG
      set_fact:
        k8s_auth_kubeconfig: "{{ lookup('env', 'KUBECONFIG') }}"
    - name: Get an existing Service object
      k8s_info:
        api_version: v1
        kind: Pod
      register: pod_list
    - name: Display k8s Cluster details
      debug:
        msg: "{{ pod_list }}"

```