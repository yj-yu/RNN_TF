name: inverse
class: center, middle, inverse
layout: true

---
class: titlepage, no-number

# Recurrent Neural Network using TensorFlow
## .gray.author[Youngjae Yu]
### .x-small[https://yj-yu.github.io/rnntf]
.bottom.center.img-66[ ![](images/snu-logo.png) ]

---
layout: false

## About

- Contents
  - RNN Basics
  - Tensorflow Basics
  - Tensorflow APIs for RNN
  - Practice - RNN
  - Practice - Monitoring
  - Advanced
  - Advanced Practice - char rnn

---

template: inverse

# RNN Basics

---
## Download first!

Download lecture resources

### https://drive.google.com/open?id=0B6JnKeZoakNCUDV2TExLVm5aS0U

Google Drive 링크로 다운로드 받으신 후 들어가주세요.
되도록 메인 디렉토리 상위에 한글 경로가 없게 해 주세요. 특정 컴퓨터 환경에서는 tensorboard 실행에 문제가 생기기도 합니다.


```python
cd tensorflow-rnn
pip install jupyter # 만약 이전에 설치하지 않으셨다면 설치해주세
pip install matplotlib # data를 시각화하기 위한 라이브러리입니다
jupyter notebook

```

혹은 git이 되시는 분들은..
### git clone https://github.com/yj-yu/tensorflow-rnn.git


---

## Recurrent Neural Networks!

.center.img-66[![](images/rl/concept.png)]

- RNNs tie the weights at .red[each time]
- Condition the neural network on .red[all previous words]
- RAM requirement only scales with number of words


.footnote[((Slide credit: cs224d)]
---

## RNN Basics

.center.img-100[![](images/rl/rnn1.png)]

.footnote[(Image credit: Karpathy’s blog)]
---
## RNN Basics

.center.img-100[![](images/rl/rnn2.png)]

.footnote[(Image credit: Karpathy’s blog)]
---
## RNN Basics

.center.img-100[![](images/rl/rnn3.png)]

.footnote[(Image credit: Karpathy’s blog)]
---
## RNN Basics

.center.img-100[![](images/rl/rnn4.png)]

.footnote[(Image credit: Karpathy’s blog)]
---
## RNN Basics

.center.img-100[![](images/rl/rnn5.png)]

.footnote[(Image credit: Karpathy’s blog)]
---


## RNN Basics

.center.img-60[![](images/rl/rnn6.png)]


---
## RNN Basics

.center.img-100[![](images/rl/rnn7.png)]


---
## RNN Basics

.center.img-100[![](images/rl/rnn8.png)]


---
##RNN Basic
.center.img-100[![rnn_cs231n](images/rnn/11.png)]

---
##RNN Basic
.center.img-100[![rnn_cs231n](images/rnn/12.png)]

---
##RNN Basic
.center.img-100[![rnn_cs231n](images/rnn/13.png)]

---
##RNN Basic
.center.img-100[![rnn_cs231n](images/rnn/14.png)]

---
##RNN Basic
.center.img-100[![rnn_cs231n](images/rnn/15.png)]

---
##RNN Basic
.center.img-100[![rnn_cs231n](images/rnn/16.png)]

---
### Time step and Batch
- Time steps are defined by **sequences length**
- The batch size is defined by **user**

### Time major representation
- `[max_timestep * batch_size * dimension]`

### RNNs typically use time major representation
```python
outputs = []
#Shape of sentences_in_batch [max_timestep * batch_size * dimension]
*for words_in_batch in sentences_in_batch:
  # Shape of words_in_batch [batch_size * dimension]
  some operations here...
  outputs.append(i-th words_output)
#Shape of outputs [max_timestep * batch_size * dimension]
print outputs

```
---




## RNN Cells (Memory Units)

- RNN basic code

.center.img-100[![](images/rl/rnn_cell_code.png)]

- RNN Cell
.center.img-20[![](images/rl/rnn_cell.png)]
.center_tex_l[
\[h\_{t+1} = tanh(x\_{t} W + h\_{t} U + b) \]
]

.footnote[(Image credit: Colah's blog)]
---


## RNN Cells (Memory Units)

- LSTM Cell
.center.img-30[![](images/rl/LSTM_cell.png)]


.center_tex[
$$
\begin{align}
i & = \sigma(x\_tU^i + s\_{t-1} W^i)  \\\\
f & = \sigma(x\_t U^f +s\_{t-1} W^f)  \\\\
o & = \sigma(x\_t U^o + s\_{t-1} W^o)  \\\\
g & = \tanh(x\_t U^g + s\_{t-1}W^g)  \\\\
c\_t & = c\_{t-1} \circ f + g \circ i  \\\\
s\_t & = \tanh(c\_t) \circ o \\\\
\end{align}
$$
]

.footnote[(Image credit: Colah's blog)]
---

## RNN Cells (Memory Units)

- GRU Cell
.center.img-33[![](images/rl/gru_cell.png)]

.center_tex[
$$
\begin{align}
z & = \sigma(x\_tU^z + s\_{t-1} W^z) \\\\
r & = \sigma(x\_t U^r +s\_{t-1} W^r)\\\\
h & = tanh(x\_t U^h + (s\_{t-1} \circ r) W^h)\\\\
s\_t & = (1 - z) \circ h + z \circ s\_{t-1} \\\\
\end{align}
$$
]



.footnote[(Image credit: Colah's blog)]
---

## RNN Loss and Optimization

###Simple RNN
.center_tex_l[
$$
\begin{align}
s\_t & = \tanh ( U x\_t + W s\_{t-1} ) \\\\
\hat{y\_t} & = softmax ( V s\_t ) \\\\
\end{align}
$$
]

###Cross Entropy Loss

.center_tex_l[
$$
\begin{align}
E(y\_t, \hat{y\_t}) & = -y\_t \log{\hat{y\_t}} \\\\
E(y, \hat{y}) & = -\sum\_{t}{E\_t (y\_t, \hat{y\_t})} \\\\
& = -\sum\_t -y\_t \log \hat{y\_t} \\\\
\end{align}
$$
]

---

## RNN Loss and Optimization

###BackProp

--
.center.img-50.marginbottom15[![](images/rnn_basic/rnn-bptt.png)]

--
.center.img-50.marginbottom15[![](images/rnn_basic/rnn-bptt-grad.png)]


---

template: inverse

# Tensorflow Basics

.sklogobg[ ![Sklogo](images/snu-logo.png) ]

---
## Tensorflow Basics
### (1) Fetch tensors via `Session.run()`

```python
x = tf.placeholder(tf.float32)
y = tf.placeholder(tf.float32)
bias = tf.Variable(1.0)

y_pred = x ** 2 + bias     # x -> x^2 + bias
loss = (y - y_pred)**2     # l2 loss?

# Error: to compute loss, y is required as a dependency
print('Loss(x,y) = %.3f' % session.run(loss, {x: 3.0}))

# OK, print 1.000 = (3**2 + 1 - 9)**2
print('Loss(x,y) = %.3f' % session.run(loss, {x: 3.0, y: 9.0}))

```
---
## Tensorflow Basics
### (2) Basic operations

```python
'''Permuting batch_size and n_steps'''
x = tf.transpose(x, [1, 0, 2])

'''Reshape to (n_steps*batch_size, n_input)'''
x = tf.reshape(x, [-1, n_input])

'''Split to get a list of 'n_steps' tensors of shape (batch_size, n_input)'''
x = tf.split(x, n_steps, 0)

'''Matrix multiplication and bias addition'''
y = tf.matmul(outputs[-1], weights['out']) + biases['out']

'''Data type casting'''
correct_pred = tf.cast(correct_pred, tf.float32)

'''Calculate mean'''
correct_pred = tf.reduce_mean(correct_pred)

'''Adam optimizer''
tf.train.AdamOptimizer(learning_rate=learning_rate)

```
---
## Tensorflow Basics
### (3) Build, Train, Test

```python
# Hyper Parameters
learning_rate = 0.001
```
--
```python
# Network Parameters
n_input = 28 # MNIST data input (img shape: 28*28)
```
--
```python
# tf Graph input
x = tf.placeholder("float", [None, n_steps, n_input])

```
--
```python
# Define weights
weights = {
  'out': tf.Variable(tf.random_normal([2*n_hidden, n_classes]))
}

```
--
```python
# Define model
def Model(x, weights, biases):
  Some ops

```
---
## Tensorflow Basics
### (3) Build, Train, Test

```python
# Get prediction from model
pred = Model(x, weights, biases)

```
--

```python
# Define loss and optimizer
cost = tf.nn.softmax_cross_entropy_with_logits(logits=pred, labels=y)
optimizer = tf.train.AdamOptimizer(learning_rate).minimize(cost)

```
--
```python
# Evaluate model
correct_pred = tf.equal(tf.argmax(pred,1), tf.argmax(y,1))
accuracy = tf.reduce_mean(tf.cast(correct_pred, tf.float32))

```
--
```python
# Initializing the variables
init = tf.initialize_all_variables()

```
---

## Tensorflow Basics
### (4) Document



## [Tensorflow APIs](https://www.tensorflow.org/api_docs/)

## [Tensorflow tutorials](https://www.tensorflow.org/tutorials/)

## [RNN tutorials](https://www.tensorflow.org/tutorials/recurrent)
---


template: inverse

# Tensorflow APIs for RNN

.sklogobg[ ![Sklogo](images/snu-logo.png) ]

---
#RNN Cells
### Pre-defined class how to calculate the output
```python
def call_cell(inputs, state):
  """Most basic RNN"""
  output = new_state = activation(W * input + U * state + B)
  return output
```
- tf.contrib.rnn.RNNCell()
- tf.contrib.rnn.LSTMCell()
- tf.contrib.rnn.GRUCell()

---

### Where do I have to call `call_cell()`
```python
outputs = []
for words_in_batch in sentences_in_batch:
  # Shape of words_in_batch [batch_size * dimension]
* some operations here...
  outputs.append(i-th words_output)
#Shape of outputs [max_timestep * batch_size * dimension]
print outputs

```
### Using RNN wrapper
```python
# Define a lstm cell with tensorflow
lstm_cell = rnn_cell.BasicLSTMCell(n_hidden, forget_bias=1.0)

# Get lstm cell output of RNN
outputs, states = rnn.static_rnn(lstm_cell, x)

# Get lstm cell output of BRNN
outputs, output_state_fw, output_state_bw = rnn.static_bidirectional_rnn(lstm_fw_cell, lstm_bw_cell, x)

```
---

### RNN Loss
```python
#Activation of the last fully connected layer
pred = tf.matmul(outputs[-1], weights['out']) + biases['out']

#Softmax cross entropy loss
#Internally calculate softmax
tf.nn.softmax_cross_entropy_with_logits(logits=pred, labels=y)

```
---

template: inverse

# Let's Do It

### .x-small[1_practice_rnn.ipynb]
.sklogobg[ ![Sklogo](images/snu-logo.png) ]

---

## MNIST Dataset

- Hand written digits

- 10 classes

- 28 \* 28 size images

- Labels are one-hot encoded

.center.img-45[![](images/mnist.png)]
---
## MNIST Dataset

.center.img-90[![](images/rl/RNN-truncated-input-output.png)]
---

template: inverse

# Monitoring
### .x-small[2_practice_monitoring.ipynb]

.sklogobg[ ![Sklogo](images/snu-logo.png) ]

---

###Visualizing Log Data with TensorBoard


.center.img-100.marginbottom15[![](images/tensorboard.png)]

---

###Visualizing Log Data with TensorBoard


```shell
'''Run tensorboard'''
$tensorboard --logdir=./ --port 1234

```

```python
# Create a summary to monitor cost tensor
tf.summary.scalar("loss", cost)

# Create a summary to monitor accuracy tensor
tf.summary.scalar("accuracy", accuracy)

# Merge all summaries into a single op
merged_summary_op = tf.summary.merge_all()

# op to write logs to Tensorboard
summary_writer = tf.train.SummaryWriter(logs_path, graph=tf.get_default_graph())

# Write logs at every iteration
summary_writer.add_summary(summary, epoch * total_batch + i)
```

---

###Visualizing Log Data with TensorBoard


```python

# Define loss and optimizer
cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits(pred, y))
optimizer = tf.train.AdamOptimizer(learning_rate=learning_rate).minimize(cost)

# Evaluate model
correct_pred = tf.equal(tf.argmax(pred,1), tf.argmax(y,1))
accuracy = tf.reduce_mean(tf.cast(correct_pred, tf.float32))

# Initializing the variables
init = tf.initialize_all_variables()

# Create a summary to monitor cost tensor
*tf.summary.scalar("loss", cost)

# Create a summary to monitor accuracy tensor
*tf.summary.scalar("accuracy", accuracy)

# Merge all summaries into a single op
*merged_summary_op = tf.summary.merge_all()

```
---


template: inverse

# Advanced

.sklogobg[ ![Sklogo](images/snu-logo.png) ]

---


##Difficulty of training RNNs
.center.img-36[![](images/longhard/eq1.png)]
<center>Fig. Schematic of a simple RNN</center>
</br>
</br>

.center.img-36[![](images/longhard/eq2.png)]
.center.img-60[![](images/longhard/eq3.png)]

---

##Difficulty of training RNNs

</br>
- For RNN training, gradients can be exploding or vanishing.
<br>
.center.img-75[![](images/longhard/eq10.png)]

</br>
---

##Difficulty of training RNNs
<!--.center[$\frac{\partial \mathcal{E}_{t}}{\partial \theta} = \sum_{1}^{n}$] -->
###Previous solutions
- Using L1 or L2 penalty on the recurrent weights
    - It can help exploding gradients, but not for vanishing.
    - This approach limits the model to a simple regime, where any information inserted in the model has to die out exponentially fast in time.
- Memory unit (ex LSTM or GRU)
    - This can help vanishing gradients.
    - This approach does not address explicitly the exploding gradients problem.

---

##Difficulty of training RNNs
<!--.center[$\frac{\partial \mathcal{E}_{t}}{\partial \theta} = \sum_{1}^{n}$] -->
###Gradient clipping

.center.img-45[![](images/longhard/eq12.png)]
<center style="font-size:20px">Alg. Gradient clipping</center>

Simple mechanism to deal with a sudden increase in the norm of the gradients is to rescale them whenever they go over a threshold.

- In experiments, training is not very sensitive to this hyperparameter and the algorithm behaves well even for rather small thresholds.

---
##Gradient Clipping

```python
'''Don't use optimizer.minimize()'''

#Calculate gradients
grads = optimizer.compute_gradients(loss)

#Gradient Clipping
clipped_grads_and_vars = [(tf.clip_by_norm(gv[0], self.max_grad_norm), gv[1]) for gv in grads]

#Apply Gradient
optim = optimizer.apply_gradients(clipped_grads_and_vars, global_step=self.global_step)
```

---

template: inverse

# Practice : character Level RNN

### .x-small[3_char_rnn_train.ipynb, 3_char_rnn_inference.ipynb]
.sklogobg[ ![Sklogo](images/snu-logo.png) ]

---
## Algorithm mimics linux kernel code
- Char RNN architecture

.center.img-50[![](images/rl/crnn.png)]

- Train Linux kernel code
- RNN learns code pattern in character level


.footnote[Reference:Tensorflow-101, Karpathy's blog]
---

## 3_char_rnn_train.ipynb

- Load linux kernel code

```python
# Load text
data_dir    = "data/linux_kernel"
save_dir    = "data/linux_kernel"
input_file  = os.path.join(data_dir, "input.txt")
with open(input_file, "r") as f:
    data = f.read()
print ("Text loaded from '%s'" % (input_file))
```
---

## And preprocess text

```python
# Preprocess Text
# First, count the number of characters
counter = collections.Counter(data)
count_pairs = sorted(counter.items(), key=lambda x: -x[1]) # <= Sort
print ("Type of 'counter.items()' is %s and length is %d"
       % (type(counter.items()), len(counter.items())))
for i in range(5):
    print ("[%d/%d]" % (i, 3)), # <= This comma remove '\n'
    print (list(counter.items())[i])

print (" ")
print ("Type of 'count_pairs' is %s and length is %d"
       % (type(count_pairs), len(count_pairs)))
for i in range(5):
    print ("[%d/%d]" % (i, 3)), # <= This comma remove '\n'
    print (count_pairs[i])
```

---

## Make Dictionary and Vocabulary.

- Map Character to digit (index)

```python
# Let's make dictionary
chars, counts = zip(*count_pairs)
vocab = dict(zip(chars, range(len(chars))))
print ("Type of 'chars' is %s and length is %d"
    % (type(chars), len(chars)))
for i in range(5):
    print ("[%d/%d]" % (i, 3)), # <= This comma remove '\n'
    print ("chars[%d] is '%s'" % (i, chars[i]))

print ("")

print ("Type of 'vocab' is %s and length is %d"
    % (type(vocab), len(vocab)))
for i in range(5):
    print ("[%d/%d]" % (i, 3)), # <= This comma remove '\n'
    print ("vocab['%s'] is %s" % (chars[i], vocab[chars[i]]))

# SAve chars and vocab
with open(os.path.join(save_dir, 'chars_vocab.pkl'), 'wb') as f:
    cPickle.dump((chars, vocab), f)
```

---
## Make Dictionary and Vocabulary.

- Map Character to digit (index)


```
Type of 'chars' is <type 'tuple'> and length is 99
[0/3] chars[0] is ' '
[1/3] chars[1] is 'e'
[2/3] chars[2] is 't'
[3/3] chars[3] is 'r'
[4/3] chars[4] is 'i'

Type of 'vocab' is <type 'dict'> and length is 99
[0/3] vocab[' '] is 0
[1/3] vocab['e'] is 1
[2/3] vocab['t'] is 2
[3/3] vocab['r'] is 3
[4/3] vocab['i'] is 4
```
---

- Converts index to char

```python

# Now convert all text to index using vocab!
corpus = np.array(list(map(vocab.get, data)))
print ("Type of 'corpus' is %s, shape is %s, and length is %d"
    % (type(corpus), corpus.shape, len(corpus)))

check_len = 10
print ("\n'corpus' looks like %s" % (corpus[0:check_len]))
for i in range(check_len):
    _wordidx = corpus[i]
    print ("[%d/%d] chars[%02d] corresponds to '%s'"
           % (i, check_len, _wordidx, chars[_wordidx]))
```

```
Type of 'corpus' is <type 'numpy.ndarray'>, shape is (1708871,), and length is 1708871

'corpus' looks like [36 22  7  0 22  0  0 13  4  8]
[0/10] chars[36] corresponds to '/'
[1/10] chars[22] corresponds to '*'
[2/10] chars[07] corresponds to '
```
---
## Generate Batch data

```python
# Generate batch data
batch_size  = 50
seq_length  = 200
num_batches = int(corpus.size / (batch_size * seq_length))
# First, reduce the length of corpus to fit batch_size
corpus_reduced = corpus[:(num_batches*batch_size*seq_length)]
xdata = corpus_reduced
ydata = np.copy(xdata)
ydata[:-1] = xdata[1:]
ydata[-1]  = xdata[0]
...
```
```
xdata is ... [36 22  7 ..., 11 25  3] and length is 1700000
ydata is ... [22  7  0 ..., 25  3 36] and length is 1700000

Type of 'xbatches' is <type 'list'> and length is 170
Type of 'ybatches' is <type 'list'> and length is 170

Type of 'temp' is <type 'list'> and length is 5
Type of 'temp[0]' is <type 'numpy.ndarray'> and shape is (50, 200)
```

---
## Now, we are ready to make our RNN model

```python
# Important RNN parameters
vocab_size = len(vocab)
rnn_size   = 128
num_layers = 2
grad_clip  = 5.

def unit_cell():
    return tf.contrib.rnn.BasicLSTMCell(rnn_size,state_is_tuple=True,reuse=tf.get_variable_scope().reuse)
cell = tf.contrib.rnn.MultiRNNCell([unit_cell() for _ in range(num_layers)])

input_data = tf.placeholder(tf.int32, [batch_size, seq_length])
targets    = tf.placeholder(tf.int32, [batch_size, seq_length])
istate     = cell.zero_state(batch_size, tf.float32)
# Weigths
with tf.variable_scope('rnnlm'):
    softmax_w = tf.get_variable("softmax_w", [rnn_size, vocab_size])
    softmax_b = tf.get_variable("softmax_b", [vocab_size])
    with tf.device("/cpu:0"):
        embedding = tf.get_variable("embedding", [vocab_size, rnn_size])
        inputs = tf.split(tf.nn.embedding_lookup(embedding, input_data), seq_length, 1)
        inputs = [tf.squeeze(_input, [1]) for _input in inputs]
```
---
```python
# Output
def loop(prev, _):
    prev = tf.nn.xw_plus_b(prev, softmax_w, softmax_b)
    prev_symbol = tf.stop_gradient(tf.argmax(prev, 1))
    return tf.nn.embedding_lookup(embedding, prev_symbol)
"""
    loop_function: If not None, this function will be applied to the i-th output
    in order to generate the i+1-st input, and decoder_inputs will be ignored,
    except for the first element ("GO" symbol).
"""
outputs, last_state = tf.contrib.rnn.static_rnn(cell, inputs, istate
                , scope='rnnlm')
output = tf.reshape(tf.concat(outputs, 1), [-1, rnn_size])
logits = tf.nn.xw_plus_b(output, softmax_w, softmax_b)
probs  = tf.nn.softmax(logits)
# Loss
loss = tf.contrib.legacy_seq2seq.sequence_loss_by_example([logits], # Input
    [tf.reshape(targets, [-1])], # Target
    [tf.ones([batch_size * seq_length])], # Weight
    vocab_size)
# Optimizer
cost     = tf.reduce_sum(loss) / batch_size / seq_length
final_state = last_state
lr       = tf.Variable(0.0, trainable=False)
tvars    = tf.trainable_variables()
grads, _ = tf.clip_by_global_norm(tf.gradients(cost, tvars), grad_clip)
_optm    = tf.train.AdamOptimizer(lr)
optm     = _optm.apply_gradients(zip(grads, tvars))
```
---
## Training code

```python
# Train the model!
num_epochs    = 50
save_every    = 500
learning_rate = 0.002
decay_rate    = 0.97

sess = tf.Session()
sess.run(tf.initialize_all_variables())
summary_writer = tf.summary.FileWriter(save_dir, graph=sess.graph)
saver = tf.train.Saver(tf.all_variables())
init_time = time.time()
for epoch in range(num_epochs):
   # Some Training code!
```
```
Instructions for updating:
Please use tf.global_variables instead.
[0/8500] cost: 4.6006 / Each batch learning took 2.2222 sec
model saved to 'data/linux_kernel/model.ckpt'
[100/8500] cost: 3.1259 / Each batch learning took 0.3366 sec
[200/8500] cost: 2.5992 / Each batch learning took 0.3258 sec
[300/8500] cost: 2.4603 / Each batch learning took 0.3260 sec
[400/8500] cost: 2.2591 / Each batch learning took 0.3136 sec
[500/8500] cost: 2.0035 / Each batch learning took 0.3140 sec
```
---
## 3_char_rnn_inference.ipynb

- Load data (Like training script)
- Set same network

- And generate sample

```python
# Sampling function
def weighted_pick(weights):
    t = np.cumsum(weights)
    s = np.sum(weights)
    return(int(np.searchsorted(t, np.random.rand(1)*s)))

# Sample using RNN and prime characters
prime = "/* "
state = sess.run(cell.zero_state(1, tf.float32))
for char in prime[:-1]:
    x = np.zeros((1, 1))
    x[0, 0] = vocab[char]
    state = sess.run(last_state, feed_dict={input_data: x, istate:state})

# Sample 'num' characters
ret  = prime
char = prime[-1] # <= This goes IN!
num  = 1000
```
---
```python
for n in range(num):
    x = np.zeros((1, 1))
    x[0, 0] = vocab[char]
    [probsval, state] = sess.run([probs, last_state]
        , feed_dict={input_data: x, istate:state})
    p      = probsval[0]

    sample = weighted_pick(p)
    # sample = np.argmax(p)

    pred   = chars[sample]
    ret    = ret + pred
    char   = pred
```

```
Sampling Done.
___________________________________________

/* struct auditued oq,
	 struct audit_enables can in a
 * the->module */
	else /* SMP a signals, ne-time bew releas for rebining routine  SIBGR.
 *
 *  Copy: state if exits/help6Counter don't be NULL field "olp.
 */
void sysctl_sched_kobjed(&watchdalleep_state, int sys_timk_ops, *mask, struct filt, unsigned int sysctl_lock)

```

---

## More results
- Latex generation. The resulting sampled Latex almost compiles.

.center.img-100[![](images/rl/latex3.png)]

.footnote[(Image credit: Karpathy’s blog)]
---
## More results
- Latex generation. The resulting sampled Latex almost compiles.

.center.img-100[![](images/rl/latex4.png)]

.footnote[(Image credit: Karpathy’s blog)]
---


template: inverse

# Practice : (Korean) Novel Writer (Char RNN)

### .x-small[4_kor_char_rnn_train.ipynb, 4_kor_char_rnn_inference.ipynb]
.sklogobg[ ![Sklogo](images/sk-logo.png) ]

---

## RNN generate korean novel

- Train korean raw text data
- Convert korean character to sequence pattern
  - Hangulpy - https://github.com/rhobot/Hangulpy
- RNN learns sequence pattern and generate similar one.

```
"크아아아아"

드래곤중에서도 최강의 투명드래곤이 울부짓었다
투명드래곤은 졸라짱쎄서 드래곤중에서 최강이엇다
신이나 마족도 이겼따 다덤벼도 이겼따 투명드래곤은
새상에서 하나였다 어쨌든 걔가 울부짓었다

"으악 제기랄 도망가자"

발록들이 도망갔다 투명드래곤이 짱이었따
그래서 발록들은 도망간 것이다

꼐속
```

.footnote[https://github.com/sjchoi86/Tensorflow-101]
.footnote[https://github.com/bluedisk/korean_char_rnn]
---
```python
import chardet # https://github.com/chardet/chardet
from TextLoader import *
from Hangulpy import *
print ("PACKAGES LOADED")
```
Conversion Function to utf-8 format
```python
def conv_file(fromfile, tofile):
    with open(fromfile, "rb") as f:
        sample_text=f.read(10240)
    pred = chardet.detect(sample_text)
    if not pred['encoding'] in ('EUC-KR', 'UTF-8', 'CP949', 'UTF-16LE'):
        print ("WARNING! Unknown encoding! : %s = %s") % (fromfile, pred['encoding'])
        pred['encoding'] = "CP949" # 못찾으면 기본이 CP949
        formfile = fromfile + ".unknown"
    elif pred['confidence'] < 0.9:
        print ("WARNING! Unsured encofing! : %s = %s / %s")
        % (fromfile, pred['confidence'], pred['encoding'])
        formfile = fromfile + ".notsure"
    with codecs.open(fromfile, "r", encoding=pred['encoding'], errors="ignore") as f:
        with codecs.open(tofile, "w+", encoding="utf8") as t:
            all_text = f.read()
            t.write(all_text)
```

---
# Convert raw text data to utf-8 format
```python
# Downloded data
dataname = 'invisible_dragon'
# SOURCE TXT FILE
fromfile = os.path.join("data",dataname,"rawtext.txt")
# TARGET TXT FILE
tofile   = os.path.join("data",dataname,"rawtext_utf8.txt")
conv_file(fromfile, tofile)
print ("UTF8-CONVERTING DONE")
print (" [%s] IS GENERATED" % (tofile))
```
---

```python
def dump_file(filename):
    result=u"" # <= UNICODE STRING
    with codecs.open(filename,"r", encoding="UTF8") as f:
        for line in f.readlines():
            line = tuple(line)
*           result = result + decompose_text(line)
    return result

parsed_txt = dump_file(tofile).encode("utf8")
```

```
Parsing data/invisible_dragon/rawtext_utf8.txt done
여러분 재가 드디어 글을...
ㅇㅕᴥㄹㅓᴥㅂㅡㄴᴥ ㅈㅐᴥㄱㅏᴥ ㄷㅡᴥㄷㅣᴥㅇㅓᴥ ㄱㅡㄹᴥㅇㅡ�
```
---
- Generate "input.txt" (Parsed pattern)

```python
with open(os.path.join("data",dataname,"input.txt"), "w") as text_file:
    text_file.write(parsed_txt)
print ("Saved to a txt file")
print (text_file)
```
- Generate "vocab.pkl" and "data.npy"

```python
data_dir    = "data/invisible_dragon"
batch_size  = 50
seq_length  = 50
data_loader = TextLoader(data_dir, batch_size, seq_length)
```

- Shift + Enter repeat. From start to end

---
## 4_kor_char_rnn_train.ipynb

Load preprocessed dataset with text loader
```python

corpus_name = "invisible_dragon" # "nine_dreams"

data_dir    = "data/" + corpus_name
batch_size  = 10
seq_length  = 100
data_loader = TextLoader(data_dir, batch_size, seq_length)

```

---

# Generate Batch.
```python
x, y = data_loader.next_batch()
```

```
Type of 'x' is <type 'numpy.ndarray'>. Shape is (50, 50)
x looks like
[[ 7  6  1 ...,  3  0 37]
 [15 18  0 ...,  0 19  3]
 [38  3  0 ..., 61 50  7]
 ...,
 [ 0  5  3 ...,  0  2 21]
 [ 8  0  2 ..., 12  5  0]
 [ 3  1  0 ..., 21 12  0]]

Type of 'y' is <type 'numpy.ndarray'>. Shape is (50, 50)
y looks like
[[ 6  1 20 ...,  0 37  7]
 [18  0  5 ..., 19  3  4]
 [ 3  0  2 ..., 50  7  6]
 ...,
 [ 5  3  0 ...,  2 21 12]
 [ 0  2 19 ...,  5  0 15]
 [ 1  0  1 ..., 12  0 11]]
```
---
## Define a multilayer LSTM network graph

```python

rnn_size   = 512
num_layers = 3
grad_clip  = 5. # <= GRADIENT CLIPPING (PRACTICALLY IMPORTANT)
vocab_size = data_loader.vocab_size

# SELECT RNN CELL (MULTI LAYER LSTM)
def unit_cell():
    return tf.contrib.rnn.BasicLSTMCell(rnn_size,state_is_tuple=True,reuse=tf.get_variable_scope().reuse)
cell = tf.contrib.rnn.MultiRNNCell([unit_cell() for _ in range(num_layers)])

# Set paths to the graph
input_data = tf.placeholder(tf.int32, [batch_size, seq_length])
targets    = tf.placeholder(tf.int32, [batch_size, seq_length])
initial_state = cell.zero_state(batch_size, tf.float32)

# Set Network
with tf.variable_scope('rnnlm'):
    softmax_w = tf.get_variable("softmax_w", [rnn_size, vocab_size])
    softmax_b = tf.get_variable("softmax_b", [vocab_size])
    with tf.device("/cpu:0"):
        embedding = tf.get_variable("embedding", [vocab_size, rnn_size])
        inputs = tf.split(tf.nn.embedding_lookup(embedding, input_data), seq_length, 1)
        inputs = [tf.squeeze(input_, [1]) for input_ in inputs]
print ("Network ready")

```

---
## Define a function

```python
# Output of RNN
outputs, last_state = tf.contrib.rnn.static_rnn(cell,inputs, initial_state,
                                                scope='rnnlm')

output = tf.reshape(tf.concat(outputs,1), [-1, rnn_size])
logits = tf.nn.xw_plus_b(output, softmax_w, softmax_b)

# Next word probability
probs = tf.nn.softmax(logits)
print ("FUNCTIONS READY")
```

---
## Define Loss functions

```python
loss = tf.contrib.legacy_seq2seq.sequence_loss_by_example([logits], # Input
    [tf.reshape(targets, [-1])], # Target
    [tf.ones([batch_size * seq_length])], # Weight
    vocab_size)
print ("LOSS FUNCTION")

```
## Define Cost functions

```python
cost = tf.reduce_sum(loss) / batch_size / seq_length

# GRADIENT CLIPPING !
lr = tf.Variable(0.0, trainable=False) # <= LEARNING RATE
tvars = tf.trainable_variables()
grads, _ = tf.clip_by_global_norm(tf.gradients(cost, tvars), grad_clip)
_optm = tf.train.AdamOptimizer(lr)
optm = _optm.apply_gradients(zip(grads, tvars))

final_state = last_state
print ("NETWORK READY")
```
---
## Optimize Network

```python

num_epochs    = 5000
save_every    = 500
learning_rate = 0.001
decay_rate    = 0.999

save_dir = 'data/' + corpus_name
sess = tf.InteractiveSession()

sess.run(tf.initialize_all_variables())
```
---
## Run Training! (It takes very long time)

```python

summary_writer = tf.summary.FileWriter(save_dir
                    , graph=sess.graph)
saver = tf.train.Saver(tf.all_variables())
for e in range(num_epochs): # for all epochs
    # LEARNING RATE SCHEDULING
    sess.run(tf.assign(lr, learning_rate * (decay_rate ** e)))

    data_loader.reset_batch_pointer()
    state = sess.run(initial_state)
    for b in range(data_loader.num_batches):
        start = time.time()
        x, y = data_loader.next_batch()
        feed = {input_data: x, targets: y, initial_state: state}
        # Train!
        train_loss, state, _ = sess.run([cost, final_state, optm], feed)
        end = time.time()
        # PRINT
        if b % 100 == 0:
            print ("%d/%d (epoch: %d), loss: %.3f, time/batch: %.3f"
                   % (e * data_loader.num_batches + b
                    , num_epochs * data_loader.num_batches
                    , e, train_loss, end - start))
```
---
```python
        # SAVE MODEL
        if (e * data_loader.num_batches + b) % save_every == 0:
            checkpoint_path = os.path.join(save_dir, 'model.ckpt')
            saver.save(sess, checkpoint_path
                       , global_step = e * data_loader.num_batches + b)
            print("model saved to {}".format(checkpoint_path))
```

---
## 4_kor_char_rnn_inference.ipynb

Set corpus name
```python
corpus_name = "invisible_dragon" # "nine_dreams
data_dir    = "data/" + corpus_name
batch_size  = 10
seq_length  = 100
data_loader = TextLoader(data_dir, batch_size, seq_length)
# This makes "vocab.pkl" and "data.npy" in "data/nine_dreams"
#  from "data/nine_dreams/input.txt"
vocab_size = data_loader.vocab_size
vocab = data_loader.vocab
chars = data_loader.chars
print ( "type of 'data_loader' is %s, length is %d"
       % (type(data_loader.vocab), len(data_loader.vocab)) )
print ( "\n" )
print ("data_loader.vocab looks like \n%s " %
       (data_loader.vocab))
print ( "\n" )
print ( "type of 'data_loader.chars' is %s, length is %d"
       % (type(data_loader.chars), len(data_loader.chars)) )
print ( "\n" )
print ("data_loader.chars looks like \n%s " % (data_loader.chars,))
```

---
## Sample character from predicted output

```python
def sample( sess, chars, vocab, __probs, num=200, prime=u'ㅇㅗᴥㄴㅡㄹᴥ '):
    state = sess.run(cell.zero_state(1, tf.float32))
    _probs = __probs
    prime = list(prime)
    for char in prime[:-1]:
        x = np.zeros((1, 1))
        x[0, 0] = vocab[char]
        feed = {input_data: x, initial_state:state}
        [state] = sess.run([last_state], feed)

    def weighted_pick(weights):
        weights = weights / np.sum(weights)
        t = np.cumsum(weights)
        s = np.sum(weights)
        return(int(np.searchsorted(t, np.random.rand(1)*s)))

    ret = prime
    char = prime[-1]
    for n in range(num):
        x = np.zeros((1, 1))
        x[0, 0] = vocab[char]
        feed = {input_data: x, initial_state:state}
        [_probsval, state] = sess.run([_probs, final_state], feed)
        p = _probsval[0]
```
---
```python
        sample = int(np.random.choice(len(p), p=p))
        # sample = weighted_pick(p)
        # sample = np.argmax(p)
        pred = chars[sample]
        ret += pred
        char = pred
    return ret
print ("sampling function done.")
```
---
## Inference Result

```python
save_dir = 'data/' + corpus_name
*prime = decompose_text(u"투명드래곤 ")

print ("Prime Text : %s => %s" % (automata(prime), "".join(prime)))
n = 4000

sess = tf.Session()
sess.run(tf.initialize_all_variables())
saver = tf.train.Saver(tf.all_variables())
ckpt = tf.train.get_checkpoint_state(save_dir)

# load_name = u'data/nine_dreams/model.ckpt-0'
load_name = os.path.join(save_dir,'model.ckpt-1900')

print (load_name)

if ckpt and ckpt.model_checkpoint_path:
    saver.restore(sess, load_name)
    sampled_text = sample(sess, chars, vocab, probs, n, prime)
    #print ("")
#     print (u"SAMPLED TEXT = %s" % sampled_text)
#     print ("")
    print ("-- RESULT --")
    print (automata("".join(sampled_text)))
```
---
## Inference Result
- Step 2500

```
투명드래곤 선봇랐터 조노 눅웅가다 가다망어흔

 즈시가 진장간 타어져 질롱해드름대 뒤크은 그기각드어레 무됨데~?~

수저질개널 진따 은이얼에주야나틔ㅕㅏㅆ다!.

"투명드래곤오 시봑임언당.


뚜크이~!!!

티멍드른곤언 셚ㄱㅂㄹ에 수대아오가젹핵말타 숙테몋에 ?~
삽승 때개근!

셰복 막5ㅋ~ㄹㅋㄱ 바터리자기짬툐 투명드래곤았본비잇다
야 처차들을 이개이사 하누빠니리 실낵나가 함산의 지치 망"
```
---
## Inference Result
- Step 7000

```
투명드래곤이 이버네요 향마신아코 본해서랄투명드래곤하고 자힐을었다

"케케케케"
바겹지가 적대시 쪼 굓굖

하지묀...

나항 졸라 중남을 쳐참다
쾰여냐! 넌 니제 고치돵이로 날아조함네 굖굖교

"이전. 제계볼뽀태 이죄가끄케"
퐈싼히하들만 투명드래곤이...

꼐속
```

---
## Try other corpus!

- Set .red[corpus_name] variable to 'nine_dreams' dataset.
  - Both train and inference scripts.
- Let's download any text data and try to train RNN!
- Larger corpus gives better result. But training time is much longer.
---
## Inference Result - nine_dreams
- Epoch 99000

```
오늘 미엿한 즉기고치를 면파지 못하고 마침께 수량궁처래마마)에
분행하니 부처님께서는 소유가 시녀 들어가 재주를
만나니 말을 건골이도, 양한림의 천재 다만의 암을 물함과 세 살이 술있다 남이
금강제의 슬픔을 떨치어 꽃고 진생을 따라가섰다.
  "시비 크게 연화봉으로 돌아와 감화록 선비의 소설해지고 이 노후의 공중을 서서함이 도적을 치었다.
  양한림이 칵을 수 일면서 의심하였나이다."
  양원수가 들으려 다시 중에서는 내전하를 부적없이를 언제하여 계랑과 더불어 딸이 들와 돌아밫히 여찌하고 황상께서 나음을 마침내
꾸짖어서 펴로 어떱게 여겨 공경에 지방 읊으셨으니 어찌(삼 되는 말이옵니까?"
  이를 경공자들이 참은케 눌러져기 크리리오?"
  하고, 이곳으로벼 생각하며 눈우면제신 1틍해 배색가룰맘이 어찌 위남여 도리에서는
크게 궁기쁜 두 구성과 더불어 가처를 감추고
있었다.
  두 시뱅과 정소저, 전교, 조정이 백릉 둘이 궁경에는 아생과 더불어
가하로자 생각하시기에 낭자가 이미 족자리로 보여 있는데 손을 잡시 명세코 별 한 손)을 듣고노며,
한림은 여중 곹오는 사음에 침녀 무매로다 인사들이 대사께 돌아가도록 하라."
  이 아가서는 새 돛아오니, 어찌 가히 변천학장, 해로다."
  하는데, 전일에 용명을 튼 보해하며 가약을 이루거늘 말하기
대부인을 목안하샀던 것에 그 뮤은 꿇어였다.

```


---







name: last-page
class: center, middle, no-number
## Thank You!
#### [@youngjae yu][yj-yu]


<div style="position:absolute; left:0; bottom:20px; padding: 25px;">
  <p class="left" style="margin:0; font-size: 13pt;">
  <b>Special Thanks to</b>: Byeongchang Kim, Jongwook Choi, Cesc Park, Sungjoon Choi </p>
</div>

.footnote[Slideshow created using [remark](http://github.com/gnab/remark).]

[yj-yu]: https://github.com/yj-yu


<!-- vim: set ft=markdown: -->
