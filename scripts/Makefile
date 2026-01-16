SHELL := /bin/bash
MAKEFLAGS += --no-print-directory

MODEL_FILE_PREFIX?=Modelfile.

.PHONY: all
all: help

.PHONY: help
help:
	@echo "Usage: make <target> [MODEL_FILE=$(MODEL_FILE_PREFIX)model]"
	@echo ""
	@echo "Targets:"
	@echo "  help           Show this help message."
	@echo "  create         Creates a model from a Modelfile (Requires the MODEL_FILE variable)"
	@echo "                 IE: make create MODEL_FILE=$(MODEL_FILE_PREFIX)myllama"
	@echo "  remove         Removes a specific model (Requires the MODEL_FILE variable)"
	@echo "                 IE: make remove MODEL_FILE=$(MODEL_FILE_PREFIX)myllama"
	@echo "  show           Show model information (Requires the MODEL_FILE variable)"
	@echo "                 IE: make show MODEL_FILE=$(MODEL_FILE_PREFIX)myllama"
	@echo "  test           Tests \"$(TEXT)\" towards \"$(ASPECT)\" with the model (Requires the MODEL_FILE variable, takes optionally: TEXT and ASPECT)"
	@echo "                 IE: make test MODEL_FILE=$(MODEL_FILE_PREFIX)myllama"
	@echo "  publish        Publishes a model to the ollama registry (Requires the MODEL_FILE variable)"
	@echo "                 IE: make publish MODEL_FILE=$(MODEL_FILE_PREFIX)myllama"
	@echo "  list           List all available models"
	@echo ""
	@make list

.PHONY: list
list:
	@echo "Available Models are:"
	@ls $(MODEL_FILE_PREFIX)* 2>/dev/null | sed -E 's,^$(MODEL_FILE_PREFIX)([^\n]+),MODEL_FILE=$(MODEL_FILE_PREFIX)\1 ($(MODEL_PREFIX)\1),' || echo "(No $(MODEL_PREFIX)* found)"

.PHONY: model_name
model_name:
	@if [ -z "$(MODEL_FILE)" ]; then \
		echo "Error: MODEL_FILE variable is not set"; \
		echo "Usage: make $(MAKECMDGOALS) MODEL_FILE=<filename>"; \
		exit -1; \
	fi
	@if [ ! -f "$(MODEL_FILE)" ]; then \
		echo "Error: Modelfile '$(MODEL_FILE)' not found"; \
		exit -2; \
	fi

.PHONY: create
create: model_name
	$(eval MODEL_NAME := $(MODEL_PREFIX)$(patsubst $(MODEL_FILE_PREFIX)%,%,$(MODEL_FILE)))
	@echo "Creating $(MODEL_NAME)"
	@ollama create $(MODEL_NAME) -f $(MODEL_FILE)

.PHONY: remove
remove: model_name
	$(eval MODEL_NAME := $(MODEL_PREFIX)$(patsubst $(MODEL_FILE_PREFIX)%,%,$(MODEL_FILE)))
	@ollama rm $(MODEL_NAME)

.PHONY: show
show: model_name
	$(eval MODEL_NAME := $(MODEL_PREFIX)$(patsubst $(MODEL_FILE_PREFIX)%,%,$(MODEL_FILE)))
	@ollama show $(MODEL_NAME)

.PHONY: test
test: model_name
	@make show > /dev/null 2>&1 || make create MODEL_FILE=$(MODEL_FILE)
	$(eval MODEL_NAME := $(MODEL_PREFIX)$(patsubst $(MODEL_FILE_PREFIX)%,%,$(MODEL_FILE)))
	@echo "The $(ACTION) \"$(TEXT)\" using $(MODEL_NAME) is => " && curl -s http://localhost:11434/api/generate -d \
		'{ "model": "$(MODEL_NAME)", "prompt": "$(TEXT)", "stream": false, "format": "$(FORMAT)" }' $(PARSER)
	@echo "The $(ACTION) \"$(TEXT)\" $(ASPECT_TERM) \"$(ASPECT)\" using $(MODEL_NAME) is => " && curl -s http://localhost:11434/api/generate -d \
		'{ "model": "$(MODEL_NAME)", "prompt": "<$(ASPECT)>$(TEXT)", "stream": false, "format": "$(FORMAT)" }' $(PARSER)

.PHONY: publish
publish: test
	$(eval MODEL_NAME := $(MODEL_PREFIX)$(patsubst $(MODEL_FILE_PREFIX)%,%,$(MODEL_FILE)))
	@echo "Publishing $(MODEL_NAME)"
	@ollama push $(MODEL_NAME)
