
# 1. Definition 

Embedding is a process that transforms discrete variables (like token) into dense vector. Through this process, semantically similar inputs are mapped to neighbouring positions within a geometric space.

Example. Cat token id: [999]. The embedding turn this token variable into a multi-dimensional vector. There are multiple dimensions like: 1. is it a living creature? 2. is it big? 3. is it a technology product?
for example we have several words, cat, elephant, phone.
cat: [0.9, -0.7, -0.9]
dog: [0.9, -0.6, -0.9]
elephant: [0.9, 0.9, -0.9]
phone: [-0.9, -0.9, 0.9]

Cat and dog are similar, so they are close in geometric space. But phone can be so far away from these two. Similar semantic meaning means closer geometric space. And it can also do mathmetical operation:
```
King - Man + Woman = Queen
```
 

