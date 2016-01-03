# DroidBrain
a number N of neural networks interacting with a central neural network

--------------

The structure is built under the following hypotheses/frameworks:
- neural networks are the basis for processing inputs
- the activation function is linear (inputsIn/inputsRel) with a sigmoid applied through float to integer conversion (0.0..1.0 -> [0,1])
- the learning strategy is based on whether or not a neuron's ouput should be related to one's input (on a scan forward basis)
- inputsIn = number of activated inputs, inputRel = number of Relevant inputs (as determined by the probability of being related)
- some neural networks only process input, others only produce output, a central network does both
- total # of related neurons could be bounded by a neighbourhood in order to reduce false assumptions. If all inputs take from all outputs then a collosal redundent brain could occur.
- In a human brain, it could (might) be that any one neuron could receive input from all neurons (including the neuron's own output) in order to fire. In the case of receiving input from its own output, the neuron would either have to already be firing to remain activated or should only be activated when it has not already been fired. This is similar to the case of depending on input from any outputs to which it contributes. Such cases should be restricted but layer ordering is not a valid solution within the central network.
- The probability of a neuron A's output being related to a neuron B's input should be determined by whether or not A has fired while the rest of B's inputs have also fired.

--------------
#Structure
- the central network's inputs are from the microphone, visual, sensory inputs, and memory.
- the central network's ouputs are to the speakers, robotic movements, and memory.

Note: the use of a 'memory' may be difficult to implement or even distinguish from the weights the learning algorithm applies to them, unfortunately neural networks are purely reactive so the idea of a 'memory' as a reference point may in fact be desireable.

------------
#Design Choices
The possibilities for data structure are endless but in order to produce a speedy algorithm the following initial design choices can be made:
- neural networks will be expressed as matrices of output data (each neuron presently contains an output)
- a 2D array makes up a single layer
- for a given brain section, the number of layers equals the smallest dimension in the outputs (ie, for an image of L x H pixels in the vision section the number of layers equals min(L,H)
- each neuron in the matrix recieves input from a maximum nighbourhood number 'P' of neurons
- neuron's that are firing during a given neuron's scan are considered relavant while those that do not fire during the evaluation are not considered relavant

# TypeDefs:

  Brain => Section[]

  Section => Layer[]

  Layer => Neuron[]

  Neuron =>  [Object,Object] = (output,inputs) s.t. output = int & inputs = [[Section#,Layer#,Neuron#]] = Triple

  Triple => int[3];
  
# Evaluation Algorithm (iterative):
Code{
  for(Section s : inputs){
    for(Layer l : s){
      for(Neuron n : Layer){
        int output = process(n.snd());
        n.snd() = updateRelavants(n,l,s);
      }
    }
  }
  for(Layer l : central){
    for(Neuron n : Layer){
        int output = process(n.snd());
        n.snd() = updateRelavants(n,l,s);
      }
  }
  for(Section s : outputs){
    for(Layer l : s){
      for(Neuron n : Layer){
        int output = process(n.snd());
        n.snd() = updateRelavants(n,l,s);
      }
    }
  }
  
}
Evaluation Algorithm (in Parallel):
  private void main(){
    Thread ts = new Thread[];
    for(Section s : inputs){
      ts.add(new Thread(ProcessSection(s)));
      ts[ts.length].start();
    }
    Thread.JoinAll(ts);
  }
  public void ProcessObject(Object o){
    if(o instanceof Neuron){
        synchronize(o) {
          int[][] ns = ((Neuron)o)[1]; 
          ((Neuron)o).fst() = process(ns);
          ((Neuron)o).snd() = updateRelavants(n,l,s);
        }
    } else {
      Thread[] ts = new Thread[((Object[])o).length];
      for(Object subObject : (Object[])o){
        ts.add(new Thread(ProcessLayer(subObject)));
        ts[ts.length].start();
      }
      Thread.JoinAll(ts);
    }
  }
  public int process(Neuron[] inputs){
    float size = (float)(input.length);
    int total = 0;
    for(Neuron n : inputs){
      synchronize (n){
        total += n.output;
      }
    }
    return ((float)total)/size; //linear & float->int sigmoid here
  }
  public Neuron[] updateRelavants(Neuron n){
    \\for the last hundred times, every neuron's the +/- 5 layers of the have this relavancy
    \\return the new bests
    Neuron[] newbests =  new Neuron[];
    for(Section s : brain){
      for(Layer l : n.getLayerNeighbours()){
      
      }
    }
    
    \\
    \\
    \\the remaining code here depends on whether or not we want neurons
    \\of input sections to accept input from those in the output sections
  }
  
  
