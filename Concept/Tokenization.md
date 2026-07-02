
# 1. Definition 

Tokenization is to split the sentence into the smallest units that an AI can recognize. 

# 2. How it splits
Tokenizer will do the splitting job. It's not a simple split based on spaces. High-frequency words might form a single token (like "the", "I"), while more complex words  are broken down into prefixes and suffixes. For example, "unhappiness" might be split into "un" (means negation), happi (means joy), ness.


# 3. Token ID 

Tokenizer will then open a dictionary (Vocabulary). The dictionary contains tens of thousands of tokens, with each token assigned to a fixed token ID. 
Token.  Token ID 
I               180
like.          521
cat.          999

