one head of attention

Query 

Key

Value

Query and Key have lower dimensions compared to the original embeddings



dimension of query/key matric vs value matrix

how to reduce the dimensions of value matrix:

low rank transformation, broken up into two value matrices



self attention vs cross-attention:

cross attention is for two completely different datasets (like texts in two different languages, or scripts vs audios)



multi-headed attention: run multiple attentions in parallel

gpt3: 96 attentions inside one block

value matrix in multi-headed settings: one giant **output matrix**



overview of transformer architecture:

attention - multilayer perception - attention - multilayer perception ...

The hope is to **gain the influence from the context that is also influenced by its own context**