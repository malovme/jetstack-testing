# Copyright 2021 The cert-manager Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# based on: https://github.com/kubernetes/test-infra/blob/e4d1738d6eb8c2c00f9d90ed9e694e48f14156c5/config/prow/Makefile

.PHONY: help
help:
	@echo "Available targets:"
	@echo "  diff-config: diff the configmap and the prow config"
	@echo "  update-config: update the configmap for the prow config"
	@echo "  diff-plugins: diff the configmap and the prow plugins"
	@echo "  update-plugins: update the configmap for the prow plugins"
	@echo "  diff-prow: diff the current prow deployment against the desired state"
	@echo "  deploy-prow: deploy the prow deployment"

# This target allows you to manually update the configmap for the prow config,
# normally this is done through GitOps.
.PHONY: bootstrap-config
bootstrap-config:
	cd ../ && \
	go run sigs.k8s.io/prow/prow/cmd/config-bootstrapper@v0.0.0-20240415223539-7013691e3f35 \
		--dry-run=false \
		--source-path=. \
		--config-path=config/config.yaml \
		--plugin-config=config/plugins.yaml \
		--job-config-path=config/jobs

.PHONY: diff-prow
diff-prow:
	kubectl diff -f ./cluster/

.PHONY: deploy-prow
deploy-prow:
	kubectl apply --server-side -f ./cluster/prowjob-crd/prowjob_customresourcedefinition.yaml
	kubectl apply --server-side -f ./cluster/
