include ../scripts/Makefile

MODEL_PREFIX?=pilardi/sentiment-analysis:
TEXT?=I love my pizza without pineapples
ACTION?=sentiment of
ASPECT?=pineapples
ASPECT_TERM?=towards
FORMAT?=json
PARSER?=| jq -c ".response | fromjson"