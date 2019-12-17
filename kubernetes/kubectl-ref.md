* kubectl 자동완성 및 alias : <https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion>

```bash
$ echo 'source <(kubectl completion zsh)' >> ~/.zshrc
$ echo 'alias k=kubectl' >> ~/.zshrc
$ echo 'complete -F __start_kubectl k' >> ~/.zshrc
```

