# FSUIE: A Novel Fuzzy Span Mechanism for Universal Information Extraction
Code of long paper "FSUIE: A Novel Fuzzy Span Mechanism for Universal Information Extraction" in ACL2023

# Downloading model ckp：
```
link：https://pan.baidu.com/s/1s3CVvBat-HLgV5gIXtA1nQ 
Extracted code：2023
put the dir "model_best" in FSUIE/
```

# Data format:
The data should be integrated into a txt file containing many lines, with each line representing one piece of data and following the following format:

1.NER:

```
{
"content": "sentence", 
"result_list": [{"text": "entity1", "start": start_index, "end": end_index}, {"text": "entity2", "start": start_index, "end": end_index}], 
"prompt": "entitiy_type"
}
```
NER example
```
{
"content": "In other violence, a Palestinian worker was shot dead in the Gaza Strip and in the West Bank, another Palestinian, a teenager, was shot by Israeli soldiers during clashes.", 
"result_list": [{"text": "West Bank", "start": 17, "end": 19}, {"text": "Gaza Strip", "start": 12, "end": 14}], 
"prompt": "location"
}
```

2.RE:

```
#For subject
{
"content": "sentence", 
"result_list": [{"text": "subject1", "start": start_index, "end": end_index}, {"text": "subject2", "start": start_index, "end": end_index}], 
"prompt": "subject"
}

#For object
{
"content": "sentence", 
"result_list": [{"text": "object1", "start": start_index, "end": end_index}, {"text": "object2", "start": start_index, "end": end_index}], 
"prompt": "object"
}

#For relation extraction
{
"content": "sentence", 
"result_list": [{"text": "object1", "start": start_index, "end": end_index}, {"text": "object2", "start": start_index, "end": end_index}], 
"prompt": "subject ## relation_type"
}
```
RE example
```
{
"content": "we 've got a strong charter school movement that i signed the legislation to get started in the state of texas.", 
"result_list": [{"text": "i", "start": 9, "end": 10}], 
"prompt": "subject"
}
{
"content": "we 've got a strong charter school movement that i signed the legislation to get started in the state of texas.", 
"result_list": [{"text": "state", "start": 18, "end": 19}], 
"prompt": "object"
}
{
"content": "we 've got a strong charter school movement that i signed the legislation to get started in the state of texas.", 
"result_list": [{"text": "state", "start": 18, "end": 19}], 
"prompt": "i ## employee-organization"
}
```


2.ASTE:

```
#For aspect
{
"content": "sentence", 
"result_list": [{"text": "aspect1", "start": start_index, "end": end_index}, {"text": "aspect2", "start": start_index, "end": end_index}], 
"prompt": "aspect"
}

#For opinion
{
"content": "sentence", 
"result_list": [{"text": "opinion1", "start": start_index, "end": end_index}, {"text": "opinion2", "start": start_index, "end": end_index}], 
"prompt": "opinion"
}

#For aspect-opinion pairs
{
"content": "sentence", 
"result_list": [{"text": "opinion1", "start": start_index, "end": end_index}], 
"prompt": "aspect'sopinion"
}

#For aspect-opinion-sentiment triplet
{
"content": "sentence", 
"result_list": [{"text": "positive", "start": -3, "end": -2}]
"prompt": "opinion aspect's Sentiment classification [negative, neutral, positive]"
}
or
{
"content": "sentence", 
"result_list": [{"text": "neutral", "start": -5, "end": -4}]
"prompt": "opinion aspect's Sentiment classification [negative, neutral, positive]"
}
or
{
"content": "sentence", 
"result_list": [{"text": "negative", "start": -7, "end": -6}]
"prompt": "opinion aspect's Sentiment classification [negative, neutral, positive]"
}
```
ASTE example
```
{
"content": "Boot time is super fast, around anywhere from 35 seconds to 1 minute.", 
"result_list": [{"text": "Boot time", "start": 0, "end": 2}], 
"prompt": "Aspect"
}
{
"content": "Boot time is super fast, around anywhere from 35 seconds to 1 minute.", 
"result_list": [{"text": "fast", "start": 4, "end": 5}], 
"prompt": "Opinion"
}
{
"content": "Boot time is super fast, around anywhere from 35 seconds to 1 minute.", 
"result_list": [{"text": "fast", "start": 4, "end": 5}], 
"prompt": "Boot time's opinion"
}
{
"content": "Boot time is super fast, around anywhere from 35 seconds to 1 minute.", 
"result_list": [{"text": "positive", "start": -3, "end": -2}], 
"prompt": "fast Boot time's Sentiment classification [negative, neutral, positive]"
}
```

# Start training:
```
python finetune.py \
    --train_path "./train.txt" \
    --dev_path "./dev.txt" \
    --save_dir "model_saver" \
    --learning_rate 1e-5 \
    --batch_size 8 \
    --max_seq_len 512 \
    --num_epochs 50 \
    --model "./model_best" \
    --seed 1000 \
    --logging_steps 400\
    --valid_steps 400 \
    --device "gpu" \
    --max_model_num 5\
    --fsl_weight 0.01\
    --base_weight 1\
    --tokenizer "./FSUIE_tokenizer"
```

# Start evaluating:
```
python NER_eval.py \
    --test_path "./test.txt" \
    --batch_size 32 \
    --max_seq_len 512 \
    --model "./model_saver" \
    --tokenizer "./FSUIE_tokenizer"
```
