include ../scripts/Makefile

MODEL_PREFIX?=pilardi/sentiment-analysis:
TEXT?=I love my pizza without pinapples
ACTION?=sentiment of
ASPECT?=pinapples
ASPECT_TERM?=towards
FORMAT?=json
PARSER?=| jq -c ".response | fromjson"