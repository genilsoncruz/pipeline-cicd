https://kubernetes.io/docs/setup/production-environment/
https://kubernetes.io/docs/setup/best-practices/
https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/

https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

sudo apt update && sudo apt upgrade -y && sudo reboot

ssh root@1.1.1.1 -i ~/.ssh/id_rsa

ansible-playbook -i hosts k8s-kubeadm-play-book.yaml

TASK [Inicializar] ****************************************************************************************************************************************************************************************
fatal: [178.128.153.66]: FAILED! => {"changed": true, "cmd": ["kubeadm", "init"], "delta": "0:00:00.836874", "end": "2022-12-22 01:59:38.345165", "msg": "non-zero return code", "rc": 1, "start": "2022-12-22 01:59:37.508291", "stderr": "error execution phase preflight: [preflight] Some fatal errors occurred:\n\t[ERROR CRI]: container runtime is not running: output: E1222 01:59:37.926112    8133 remote_runtime.go:948] \"Status from runtime service failed\" err=\"rpc error: code = Unimplemented desc = unknown service runtime.v1alpha2.RuntimeService\"\ntime=\"2022-12-22T01:59:37Z\" level=fatal msg=\"getting status of runtime: rpc error: code = Unimplemented desc = unknown service runtime.v1alpha2.RuntimeService\"\n, error: exit status 1\n[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`\nTo see the stack trace of this error execute with --v=5 or higher", "stderr_lines": ["error execution phase preflight: [preflight] Some fatal errors occurred:", "\t[ERROR CRI]: container runtime is not running: output: E1222 01:59:37.926112    8133 remote_runtime.go:948] \"Status from runtime service failed\" err=\"rpc error: code = Unimplemented desc = unknown service runtime.v1alpha2.RuntimeService\"", "time=\"2022-12-22T01:59:37Z\" level=fatal msg=\"getting status of runtime: rpc error: code = Unimplemented desc = unknown service runtime.v1alpha2.RuntimeService\"", ", error: exit status 1", "[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`", "To see the stack trace of this error execute with --v=5 or higher"], "stdout": "[init] Using Kubernetes version: v1.26.0\n[preflight] Running pre-flight checks", "stdout_lines": ["[init] Using Kubernetes version: v1.26.0", "[preflight] Running pre-flight checks"]}
