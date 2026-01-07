### Sentence alignment related

1. In `Sentences_Oare_FirstWord_LinNum.csv`, the `first_word_number` of a piece of text is not 1.

- The transliteration starts with a number

This is the majority case.

```plaintext
79cefbac-e992-45ad-87b4-5d5f3c2976ce
9418eea8-a40f-4660-adfa-7347b30310d5
```

To test other cases:

```python
# train_pubtext_df == published_texts.csv left-join train.csv

for row in train_pubtext_df.itertuples():
    text_uuid = row.oare_id
    text = row.transliteration_orig

    matched_sentences_df = sentence_align_df[sentence_align_df['text_uuid'] == text_uuid]
    num_sentences = len(matched_sentences_df)

    if num_sentences <= 1:
        # there is no available guide for sentence alignment or there is only 1 sentence
        continue
    
    # split the sentence
    words = text.split()
    first_word_indices = matched_sentences_df['first_word_number'].values # NOTE: these are 1-indexed
    first_word_spellings = matched_sentences_df['first_word_spelling'].values

    if text_uuid in uuid_in_train and first_word_indices[0] != 1 and not words[0].isnumeric() and not ''.join(words[0].split(".")).isnumeric():
        print(text_uuid)
```

- The transliteration starts with a `{large break}`. 

```plaintext
814ad661-c264-4d94-9c1e-2273334268e6
fad027c2-6213-4e5c-bcdc-89de2e40d198
31f0a2fb-00be-4f3a-b5f5-036afa8582f8
2ab54625-1599-4175-ae15-6cdd90b0d960
```

- Missing the first sentence

```plaintext
bdf62c81-286c-407f-98f4-8674d658779b
6e81bb53-74f5-42cc-8e63-26acc396a6cd
54364e47-54dc-4022-9c48-9006a2fe1b53
4eb23b20-e6ce-4c45-ac07-07d62a2ef0d0 (bad)
4633e49c-6469-4b90-a381-a9853f1c0b9f (bad)

```

