Download Link: https://assignmentchef.com/product/solved-cse-238systems-programming-project-3
<br>









<h1>1 Logistics</h1>

This is a group project of up to two people, or you can do it individually. If you form a group, only one of the group members can upload the project but, in this case, write the name of all group members as a file name. You must run this project on a 64-bit x86-64 machine.

<h1>2 Downloading the assignment</h1>

You can download the handout for the project through Canvas. You will need the trace files in traces.rar and the RAM image in RAM.txt.

<h1>3 Description</h1>

In this project, you will write a cache simulator which takes an image of memory and a memory trace as input, simulates the hit/miss behavior of a cache memory on this trace, and outputs the total number of hits, misses, and evictions for each cache type along with the content of each cache at the end. Your simulator will take the following command-line arguments:




Usage: ./your_simulator -L1s &lt;L1s&gt; -L1E &lt;L1E&gt; -L1b &lt;L1b&gt;

-L2s &lt;L2s&gt; -L2E &lt;L2E&gt; -L2b &lt;L2b&gt;

-t &lt;tracefile&gt;




<ul>

 <li>-L1s &lt;L1s&gt;: Number of set index bits for L1 data/instruction cache (<em>S </em>= 2<em><sup>s </sup></em>is the number of sets)</li>

 <li>-L1E &lt;L1E&gt;: Associativity for L1 data/instruction cache (number of lines per set)</li>

 <li>-L1b &lt;L1b&gt;: Number of block bits for L1 data/instruction cache (<em>B </em>= 2<em><sup>b </sup></em>is the block size)</li>

 <li>-L2s &lt;L2s&gt;: Number of set index bits for L2 cache (<em>S </em>= 2<em><sup>s </sup></em>is the number of sets)</li>

 <li>-L2E &lt;L2E&gt;: Associativity for L2 cache (number of lines per set)</li>

 <li>-L2b &lt;L2b&gt;: Number of block bits for L2 cache (<em>B </em>= 2<em><sup>b </sup></em>is the block size)</li>

 <li>-t &lt;tracefile&gt;: Name of the trace file (see Reference Trace Files part below)</li>

</ul>




The command-line arguments are based on the notation (<em>s</em>, <em>E</em>, and <em>b</em>) from page 652 of the CS:APP3e textbook. The s, E, and b values will be the same for both L1 data and instruction caches.

For example, if you want to simulate a fully associative (s=0) L1 cache of 2 lines (E=2) and 8 blocks (b=3), and a 2-way set associative (E=2) L2 cache of 2 sets (s=1) and 8 blocks (b=3), and see the results for the trace file test1.trace, you will run your program with the arguments given in the following example. Also, the results should be in the given format.

linux&gt; ./your_simulator  -L1s 0 -L1E 2 -L1b 3 -L2s 1 -L2E 2 -L2b 3 -t test1.trace

L1I-hits:0 L1I-misses:1 L1I-evictions:0

L1D-hits:1 L1D-misses:1 L1D-evictions:0 L2-hits:1 L2-misses:2 L2-evictions:0




<h2>L 5, 3</h2>

L1D miss, L2 miss

Place in L2 set 0, L1D

I 10, 8

L1I miss, L2 miss

Place in L2 set 0, L1I

S 0, 1, ab

L1D hit, L2 hit   Store in L1D, L2, RAM










<h2>Programming Rules</h2>

<ul>

 <li>You can use any either Java or C or Python.</li>

 <li>Name of your source code file should include your name like <em>c, </em>(if you are group of two then both group members)</li>

 <li>Your code must compile without warnings in order to receive credit.</li>

 <li>Your simulator must work correctly for different sets of <em>s</em>, <em>E</em>, and <em>b</em> values for each cache type. This means that you will need to allocate storage for your simulator’s data structures using the malloc Type “man malloc” for information about this function.</li>

 <li>For this project, we are interested in L1 data cache, L1 instruction cache, and unified L2 cache performance.</li>

 <li>Each of the caches will implement <u>write-through and no write allocate</u> mechanism for store and modify instructions.</li>

 <li>For the <u>evictions, FIFO (first in first out)</u> policy will be used.</li>

 <li>To receive credit, for each cache (L1D, L1I, L2), you must print the total number of hits, misses, and evictions, at the end of your program.</li>

 <li>For this project, you should assume that memory accesses are aligned properly, such that a single memory access never crosses block boundaries (Read and write requests are less than or equal to block size).</li>

 <li>We will hand you sample RAM image for testing. For each Miss in the caches you can fetch the data from the text file “RAM.txt”. This file is a txt file that includes the contents of the memory in the beginning of the execution.</li>

 <li>The contents of the caches at the end of the execution will be written to the corresponding txt file for each cache.</li>

</ul>

<strong> </strong>

<h2>Reference Trace Files</h2>




The traces subdirectory of the handout directory contains a collection of <em>reference trace files </em>that we will use to evaluate the correctness of the cache simulator you write. The memory trace files have the following form:




M 000ebe20, 3, 58a35a

L 000eaa30, 6

S 0003b020, 7, abb2cdc69bb454

I 00002010, 6




Each line denotes one or two memory accesses. The format of each line for I and L:

<em>operation address, size </em>

The format of each line for M and S:

<em>operation address, size, data </em>

The <em>operation </em>field denotes the type of memory access:

<ul>

 <li>“I” denotes an instruction load,</li>

 <li>“L” a data load,</li>

 <li>“S” a data store, and</li>

 <li>“M” a data modify (i.e., a data load followed by a data store).</li>

</ul>

The <em>address </em>field specifies a 32-bit hexadecimal memory address.

The <em>size </em>field specifies the number of bytes accessed by the operation.

The <em>data </em>field specifies the data bytes stored in the given address.

<strong> </strong>

<strong>EXAMPLE RUN: </strong>test1.trace:

L 5, 3

I 10, 8  S 0, 1, ab




Inital image of RAM and caches:

<table width="690">

 <tbody>

  <tr>

   <td width="301">L1Itag               time            v   data

    <table width="278">

     <tbody>

      <tr>

       <td width="70"> </td>

       <td width="70"> </td>

       <td width="17"> </td>

       <td width="122"> </td>

      </tr>

      <tr>

       <td width="70"> </td>

       <td width="70"> </td>

       <td width="17"> </td>

       <td width="122"> </td>

      </tr>

     </tbody>

    </table> L1Dtag               time            v   data

    <table width="278">

     <tbody>

      <tr>

       <td width="70"> </td>

       <td width="70"> </td>

       <td width="17"> </td>

       <td width="122"> </td>

      </tr>

      <tr>

       <td width="70"> </td>

       <td width="70"> </td>

       <td width="17"> </td>

       <td width="122"> </td>

      </tr>

     </tbody>

    </table>   L2tag         time         v   data

    <table width="278">

     <tbody>

      <tr>

       <td rowspan="2" width="56">Set 0</td>

       <td width="56"> </td>

       <td width="56"> </td>

       <td width="14"> </td>

       <td width="98"> </td>

      </tr>

      <tr>

       <td width="56"> </td>

       <td width="56"> </td>

       <td width="14"> </td>

       <td width="98"> </td>

      </tr>

     </tbody>

    </table>


    <table width="278">

     <tbody>

      <tr>

       <td rowspan="2" width="56">Set 1</td>

       <td width="56"> </td>

       <td width="56"> </td>

       <td width="14"> </td>

       <td width="98"> </td>

      </tr>

      <tr>

       <td width="56"> </td>

       <td width="56"> </td>

       <td width="14"> </td>

       <td width="98"> </td>

      </tr>

     </tbody>

    </table></td>

   <td width="389">RAMAddress                                      Data

    <table width="366">

     <tbody>

      <tr>

       <td width="183">0x00000000</td>

       <td width="183">1234567887654321</td>

      </tr>

      <tr>

       <td width="183">0x00000008</td>

       <td width="183">8765432112345678</td>

      </tr>

      <tr>

       <td width="183">0x00000010</td>

       <td width="183">0abcd1e22e1dcba0</td>

      </tr>

      <tr>

       <td width="183">0x00000018</td>

       <td width="183">f1e2a3d44d3a2e1f</td>

      </tr>

      <tr>

       <td width="183">0x00000020</td>

       <td width="183">c1a2b3e44e3b2a1c</td>

      </tr>

      <tr>

       <td width="183">0x00000028</td>

       <td width="183">0123abcddcba3210</td>

      </tr>

      <tr>

       <td width="183">0x00000030</td>

       <td width="183">dcba12300321abcd</td>

      </tr>

      <tr>

       <td width="183">0x00000038</td>

       <td width="183">02468aceeca86420</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>







Your simulation:

linux&gt; ./your_simulator  -L1s 0 -L1E 2 -L1b 3 -L2s 1 -L2E 2 -L2b 3 -t test1.trace




L1I-hits:0 L1I-misses:1 L1I-evictions:0

L1D-hits:1 L1D-misses:1 L1D-evictions:0 L2-hits:1 L2-misses:2 L2-evictions:0




<h3>L 5, 3</h3>

L1D miss, L2 miss

Place in L2 set 0, L1D

I 10, 8

L1I miss, L2 miss

Place in L2 set 0, L1I

S 0, 1, ab

L1D hit, L2 hit   Store in L1D, L2, RAM L 5, 3  L1D miss, L2 miss

Place in L2 set 0, L1D

<table width="694">

 <tbody>

  <tr>

   <td width="305">L1Itag               time            v   data

    <table width="282">

     <tbody>

      <tr>

       <td width="69"> </td>

       <td width="70"> </td>

       <td width="17"> </td>

       <td width="126"> </td>

      </tr>

      <tr>

       <td width="69"> </td>

       <td width="70"> </td>

       <td width="17"> </td>

       <td width="126"> </td>

      </tr>

     </tbody>

    </table> L1Dtag               time            v   data

    <table width="282">

     <tbody>

      <tr>

       <td width="69">0000000</td>

       <td width="70">1</td>

       <td width="17">1</td>

       <td width="126">1234567887654321</td>

      </tr>

      <tr>

       <td width="69"> </td>

       <td width="70"> </td>

       <td width="17"> </td>

       <td width="126"> </td>

      </tr>

     </tbody>

    </table>   L2tag         time         v   data

    <table width="278">

     <tbody>

      <tr>

       <td rowspan="2" width="56">Set 0</td>

       <td width="56">0000000</td>

       <td width="56">1</td>

       <td width="14">1</td>

       <td width="98">1234567887654321</td>

      </tr>

      <tr>

       <td width="56"> </td>

       <td width="56"> </td>

       <td width="14"> </td>

       <td width="98"> </td>

      </tr>

      <tr>

       <td width="56"> </td>

       <td colspan="2" width="111"> </td>

       <td colspan="2" width="111"> </td>

      </tr>

      <tr>

       <td rowspan="2" width="56">Set 1</td>

       <td width="56"> </td>

       <td width="56"> </td>

       <td width="14"> </td>

       <td width="98"> </td>

      </tr>

      <tr>

       <td width="56"> </td>

       <td width="56"> </td>

       <td width="14"> </td>

       <td width="98"> </td>

      </tr>

     </tbody>

    </table></td>

   <td width="389">RAMAddress                                      Data

    <table width="366">

     <tbody>

      <tr>

       <td width="183">0x00000000</td>

       <td width="183">1234567887654321</td>

      </tr>

      <tr>

       <td width="183">0x00000008</td>

       <td width="183">8765432112345678</td>

      </tr>

      <tr>

       <td width="183">0x00000010</td>

       <td width="183">0abcd1e22e1dcba0</td>

      </tr>

      <tr>

       <td width="183">0x00000018</td>

       <td width="183">f1e2a3d44d3a2e1f</td>

      </tr>

      <tr>

       <td width="183">0x00000020</td>

       <td width="183">c1a2b3e44e3b2a1c</td>

      </tr>

      <tr>

       <td width="183">0x00000028</td>

       <td width="183">0123abcddcba3210</td>

      </tr>

      <tr>

       <td width="183">0x00000030</td>

       <td width="183">dcba12300321abcd</td>

      </tr>

      <tr>

       <td width="183">0x00000038</td>

       <td width="183">02468aceeca86420</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>




I 10, 8  L1I miss, L2 miss

Place in L2 set 0, L1I

<table width="694">

 <tbody>

  <tr>

   <td width="305">L1Itag               time            v   data

    <table width="282">

     <tbody>

      <tr>

       <td width="69">0000002</td>

       <td width="70">2</td>

       <td width="17">1</td>

       <td width="126">0abcd1e22e1dcba0</td>

      </tr>

      <tr>

       <td width="69"> </td>

       <td width="70"> </td>

       <td width="17"> </td>

       <td width="126"> </td>

      </tr>

     </tbody>

    </table> L1Dtag               time            v   data

    <table width="282">

     <tbody>

      <tr>

       <td width="69">0000000</td>

       <td width="70">1</td>

       <td width="17">1</td>

       <td width="126">1234567887654321</td>

      </tr>

      <tr>

       <td width="69"> </td>

       <td width="70"> </td>

       <td width="17"> </td>

       <td width="126"> </td>

      </tr>

     </tbody>

    </table>   L2tag         time         v   data

    <table width="278">

     <tbody>

      <tr>

       <td rowspan="2" width="56">Set 0</td>

       <td width="56">0000000</td>

       <td width="56">1</td>

       <td width="14">1</td>

       <td width="98">1234567887654321</td>

      </tr>

      <tr>

       <td width="56">0000001</td>

       <td width="56">2</td>

       <td width="14">1</td>

       <td width="98">0abcd1e22e1dcba0</td>

      </tr>

      <tr>

       <td width="56"> </td>

       <td colspan="2" width="111"> </td>

       <td colspan="2" width="111"> </td>

      </tr>

      <tr>

       <td rowspan="2" width="56">Set 1</td>

       <td width="56"> </td>

       <td width="56"> </td>

       <td width="14"> </td>

       <td width="98"> </td>

      </tr>

      <tr>

       <td width="56"> </td>

       <td width="56"> </td>

       <td width="14"> </td>

       <td width="98"> </td>

      </tr>

     </tbody>

    </table></td>

   <td width="389">RAMAddress                                      Data

    <table width="366">

     <tbody>

      <tr>

       <td width="183">0x00000000</td>

       <td width="183">1234567887654321</td>

      </tr>

      <tr>

       <td width="183">0x00000008</td>

       <td width="183">8765432112345678</td>

      </tr>

      <tr>

       <td width="183">0x00000010</td>

       <td width="183">0abcd1e22e1dcba0</td>

      </tr>

      <tr>

       <td width="183">0x00000018</td>

       <td width="183">f1e2a3d44d3a2e1f</td>

      </tr>

      <tr>

       <td width="183">0x00000020</td>

       <td width="183">c1a2b3e44e3b2a1c</td>

      </tr>

      <tr>

       <td width="183">0x00000028</td>

       <td width="183">0123abcddcba3210</td>

      </tr>

      <tr>

       <td width="183">0x00000030</td>

       <td width="183">dcba12300321abcd</td>

      </tr>

      <tr>

       <td width="183">0x00000038</td>

       <td width="183">02468aceeca86420</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>




S 0, 1, ab

<h3>L1D hit, L2 hit</h3>

Store in L1D, L2, RAM




<table width="694">

 <tbody>

  <tr>

   <td width="305">L1Itag               time            v   data

    <table width="282">

     <tbody>

      <tr>

       <td width="69">0000002</td>

       <td width="70">2</td>

       <td width="17">1</td>

       <td width="126">0abcd1e22e1dcba0</td>

      </tr>

      <tr>

       <td width="69"> </td>

       <td width="70"> </td>

       <td width="17"> </td>

       <td width="126"> </td>

      </tr>

     </tbody>

    </table> L1Dtag               time            v   data

    <table width="282">

     <tbody>

      <tr>

       <td width="69">0000000</td>

       <td width="70">1</td>

       <td width="17">1</td>

       <td width="126">ab34567887654321</td>

      </tr>

      <tr>

       <td width="69"> </td>

       <td width="70"> </td>

       <td width="17"> </td>

       <td width="126"> </td>

      </tr>

     </tbody>

    </table>   L2tag         time         v   data

    <table width="278">

     <tbody>

      <tr>

       <td rowspan="2" width="56">Set 0</td>

       <td width="56">0000000</td>

       <td width="56">1</td>

       <td width="14">1</td>

       <td width="98">ab34567887654321</td>

      </tr>

      <tr>

       <td width="56">0000001</td>

       <td width="56">2</td>

       <td width="14">1</td>

       <td width="98">0abcd1e22e1dcba0</td>

      </tr>

      <tr>

       <td width="56"> </td>

       <td colspan="2" width="111"> </td>

       <td colspan="2" width="111"> </td>

      </tr>

      <tr>

       <td rowspan="2" width="56">Set 1</td>

       <td width="56"> </td>

       <td width="56"> </td>

       <td width="14"> </td>

       <td width="98"> </td>

      </tr>

      <tr>

       <td width="56"> </td>

       <td width="56"> </td>

       <td width="14"> </td>

       <td width="98"> </td>

      </tr>

     </tbody>

    </table></td>

   <td width="389">RAMAddress                                      Data

    <table width="366">

     <tbody>

      <tr>

       <td width="183">0x00000000</td>

       <td width="183">ab34567887654321</td>

      </tr>

      <tr>

       <td width="183">0x00000008</td>

       <td width="183">8765432112345678</td>

      </tr>

      <tr>

       <td width="183">0x00000010</td>

       <td width="183">0abcd1e22e1dcba0</td>

      </tr>

      <tr>

       <td width="183">0x00000018</td>

       <td width="183">f1e2a3d44d3a2e1f</td>

      </tr>

      <tr>

       <td width="183">0x00000020</td>

       <td width="183">c1a2b3e44e3b2a1c</td>

      </tr>

      <tr>

       <td width="183">0x00000028</td>

       <td width="183">0123abcddcba3210</td>

      </tr>

      <tr>

       <td width="183">0x00000030</td>

       <td width="183">dcba12300321abcd</td>

      </tr>

      <tr>

       <td width="183">0x00000038</td>

       <td width="183">02468aceeca86420</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>




<h1>5 Working on the Project</h1>

Here are some hints and suggestions for working on the project:

<ul>

 <li>Each data load (L) or store (S) operation can cause at most one cache miss (Always aligned. If blocks are 4 bytes, there will be no request more than 4 bytes in test trace inputs). The data modify operation (M) is treated as a load followed by a store to the same address. Thus, an M operation can result in two cache hits, or a miss and a hit plus a possible eviction.</li>

</ul>





