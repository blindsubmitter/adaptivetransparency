# adaptivetransparency

This repository contains code and data for an anonymous submission to the ASE'17 conference.

To run the example, you can supply the filename of a numeric PRISM model, e.g.,

./r examples/Fig6

The file examples/Fig6.pm contains the specification of the Discrete Time
Markov Chain (DTMC) model (i.e., Fig. 6).  In this model, the variables w00,
w02, w11, and w21 are called "adaptive transparency" variables because they
change the perception of the probabilities from the node "s=1" (i.e., 0,1).

<pre>

dtmc

const double p1 = 0.1;
const double p2 = 0.1;
const double ir0 = 1;
const double ir1 = 1;
const double ir2 = 1;
const double iu0 = 1;
const double iu1 = 1;
const double iu2 = 1;
const double w00 = 1;
const double w02 = 1;
const double w11 = 1;
const double w21 = 1;

module COMPOSED

s: [0..9] init 0;

[]s=0->((1-p2)/2):(s'=1)+    (p2/2):(s'=2)+((1-p1)/2):(s'=3)+    (p1/2):(s'=6);
[]s=1->    (w00*(1-p2)/2):(s'=0)+(w02*p2/2):(s'=2)+(w11*(1-p1)/2):(s'=5)+    (w21*p1/2):(s'=7);
[]s=2->    (1-p1):(s'=5)+        p1:(s'=8);
[]s=3->    (p1/2):(s'=0)+((1-p2)/2):(s'=4)+    (p2/2):(s'=5)+((1-p1)/2):(s'=6);
[]s=4->    (p1/2):(s'=1)+    (p2/2):(s'=3)+((1-p2)/2):(s'=5)+((1-p1)/2):(s'=7);
[]s=5->        p1:(s'=3)+    (1-p1):(s'=8);
[]s=6->    (1-p2):(s'=7)+        p2:(s'=8);
[]s=7->        p2:(s'=6)+    (1-p2):(s'=8);

endmodule

rewards "impact"
  s=0: (ir0+iu0);
  s=1: (ir0+iu1);
  s=2: (ir0+iu2);
  s=3: (ir1+iu0);
  s=4: (ir1+iu1);
  s=5: (ir1+iu2);
  s=6: (ir2+iu0);
  s=7: (ir2+iu1);
  s=8: (ir2+iu2);
endrewards

</pre>

The above model is a result of parallel composition of two DTMC models, namely examples/User.pm and examples/Room.pm.

<pre>
dtmc

const double p2 = 0.1;
const double iu0 = 1;
const double iu1 = 1;
const double iu2 = 1;

module USER

// 0 - out of office; 1 - inside office; 2 - exit
u : [0..2] init 0;

[]u=0->(1-p2):(u'=1)+p2(u'=2);
[]u=1->p2:(u'=0)+(1-p2):(u'=2);

endmodule

rewards "user_impact"
  u=0: iu0;
  u=1: iu1;
  u=2: iu2;
endrewards
</pre>

<pre>
dtmc

const double p2 = 0.1;
const double iu0 = 1;
const double iu1 = 1;
const double iu2 = 1;

module USER

// 0 - out of office; 1 - inside office; 2 - exit
u : [0..2] init 0;

[]u=0->(1-p2):(u'=1)+p2(u'=2);
[]u=1->p2:(u'=0)+(1-p2):(u'=2);

endmodule

rewards "user_impact"
  u=0: iu0;
  u=1: iu1;
  u=2: iu2;
endrewards
</pre>
