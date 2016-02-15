

### Messing with bit syntax

6> <<_/binary>> = integer_to_binary(100000000).
<<"100000000">>

Some wrongs..

```
7> <<_/big-unsigned-integer>> = integer_to_binary(100000000).
** exception error: no match of right hand side value <<"100000000">>
8> <<_/big-unsigned-integer-unit>> = integer_to_binary(100000000).
* 1: bit type unit undefined
9> <<_/big-unsigned-integer-unit:32>> = integer_to_binary(100000000).
* 1: a bit unit size must not be specified unless a size is specified too
10> <<_:16/big-unsigned-integer-unit:16>> = integer_to_binary(100000000).
** exception error: no match of right hand side value <<"100000000">>
11> <<_:16/big-unsigned-integer-unit>> = integer_to_binary(100000000).
* 1: bit type unit undefined
12> <<_:16/big-unsigned-integer>> = integer_to_binary(100000000).
** exception error: no match of right hand side value <<"100000000">>
13> <<_:1/big-unsigned-integer>> = integer_to_binary(100000000).
** exception error: no match of right hand side value <<"100000000">>
14> <<_:32/big-unsigned-integer>> = integer_to_binary(100000000).
** exception error: no match of right hand side value <<"100000000">>
15> <<_:64/big-unsigned-integer>> = integer_to_binary(100000000).
** exception error: no match of right hand side value <<"100000000">>
16> <<_:64/big-unsigned-integer>> = integer_to_binary(1).
** exception error: no match of right hand side value <<"1">>
```

Some rights...

```
17> <<_>> = integer_to_binary(1).
<<"1">>
18> <<_/binary>> = integer_to_binary(1).
<<"1">>
20> <<_/integer>> = integer_to_binary(1).
<<"1">>
21> <<X/integer>> = integer_to_binary(1).
<<"1">>
22> X.
49
```

```
23> integer_to_binary(1).
<<"1">>

24> erlang:integer_to_list(1).
"1"

29> erlang:tuple_to_list({1,"asddda"}).
[1,"asddda"]

20: <<_/integer>> = integer_to_binary(1)
-> <<"1">>
21: <<X/integer>> = integer_to_binary(1)
-> <<"1">>
22: X
-> 49
23: integer_to_binary(1)
-> <<"1">>
24: integer_to_list(1)
-> "1"
27: lists:nth(1, integer_to_list(1))
-> 49
28: tuple_to_list({1})
-> [1]
29: tuple_to_list({1,"asddda"})
-> [1,"asddda"]
```

```
84> {ok, Socket} = gen_udp:open(25826, [binary, {active, false}]),
84> io:format("server opened socket:~p~n",[Socket]),
84> inet:setopts(Socket, [{active, once}]).
server opened socket:#Port<0.2363>
ok
85>
85> {udp, Packet} = receive
85>     {udp, Socket, Host, Port, Bin} ->
85>       io:format("server received:~p~n", [Bin]),
85>     {udp, Bin}
85>       ;
85>     Other ->
85>       io:format("server received:~p~n", [Other]),
85>       Other
85>   after 80000 -> 0
85>   end.
server received:<<0,0,0,14,108,111,99,97,108,104,111,115,116,0,0,8,0,12,21,176,
                  106,248,231,146,20,106,0,9,0,12,0,0,0,1,64,0,0,0,0,2,0,14,
                  105,110,116,101,114,102,97,99,101,0,0,3,0,10,97,119,100,108,
                  48,0,0,4,0,14,105,102,95,111,99,116,101,116,115,0,0,6,0,24,0,
                  ..... elided
                  248,231,146,24,155,0,4,0,15,105,102,95,112,97,99,107,101,116,
                  115,0,0,6,0,24,0,2,2,2,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,157,0,8,
                  0,0,0,0,0,0,0,0,0,8,0,12,21,176,106,250,39,103,140,0,0,4,0,
                  15,105,102,95,112,97,99,107,101,116,115,0,0,6,0,24,0,2,2,2,0,
                  0,0,0,0,0,0,0,0,0,0,0,0,0,0,0>>
{udp,<<0,0,0,14,108,111,99,97,108,104,111,115,116,0,0,8,
       0,12,21,176,106,248,231,146,20,106,0,...>>}
86>
86> Packet.
<<0,0,0,14,108,111,99,97,108,104,111,115,116,0,0,8,0,12,
  21,176,106,248,231,146,20,106,0,9,0,...>>

88> c(collectd_erlang_parser).
collectd_erlang_parser.erl:24: Warning: variable 'Ip' is unused
collectd_erlang_parser.erl:24: Warning: variable 'Port' is unused
{ok,collectd_erlang_parser}
89>

89> collectd_erlang_parser:parse(Packet,[]).
Packet: [values,
         [0,0],
         type,
         <<105,102,95,112,97,99,107,101,116,115,0>>,
         time,1562866693333027840,values,
         [0,0],
         type,
         <<105,102,95,111,99,116,101,116,115,0>>,
         plugin_instance,
         <<103,105,102,48,0>>,
         time,1562866693332977374,values,
         [0,0],
         type_instance,
         <<0>>,
         type,
         <<105,102,95,101,114,114,111,114,115,0>>,
         plugin_instance,
         <<108,111,48,0>>,
         plugin,
         <<105,110,116,101,114,102,97,99,101,0>>,
         time,1562866693332883958,values,
         [2546405376.0],
         type_instance,
         <<102,114,101,101,0>>,
         type,
         <<109,101,109,111,114,121,0>>,
         plugin_instance,
         <<0>>,
         plugin,
         <<109,101,109,111,114,121,0>>,
         time,1562866693332663841,values,
         [69725,69725],
         type_instance,
         <<0>>,
         type,
         <<105,102,95,112,97,99,107,101,116,115,0>>,
         plugin_instance,
         <<108,111,48,0>>,
         plugin,
         <<105,110,116,101,114,102,97,99,101,0>>,
         time,1562866693332809870,values,
         [2743644160.0],
         type_instance,
         <<105,110,97,99,116,105,118,101,0>>,
         type,
         <<109,101,109,111,114,121,0>>,
         plugin_instance,
         <<0>>,
         plugin,
         <<109,101,109,111,114,121,0>>,
         time,1562866693332663841,values,
         [9195837,9195837],
         type_instance,
         <<0>>,
         type,
         <<105,102,95,111,99,116,101,116,115,0>>,
         plugin_instance,
         <<108,111,48,0>>,
         plugin,
         <<105,110,116,101,114,102,97,99,101,0>>,
         time,1562866693332726118,values,
         [8286101504.0],
         type_instance,
         <<97,99,116,105,118,101,0>>,
         type,
         <<109,101,109,111,114,121,0>>,
         plugin,
         <<109,101,109,111,114,121,0>>,
         time,1562866693332663841,values,
         [1.86181640625,1.9248046875,2.134765625],
         type_instance,
         <<0>>,
         type,
         <<108,111,97,100,0>>,
         plugin,
         <<108,111,97,100,0>>,
         time,1562866693332684242,values,
         [3125235712.0],
         type_instance,
         <<119,105,114,101,100,0>>,
         type,
         <<109,101,109,111,114,121,0>>,
         plugin_instance,
         <<0>>,
         plugin,
         <<109,101,109,111,114,121,0>>,
         time,1562866693332663841,values,
         [0,0],
         type,
         <<105,102,95,101,114,114,111,114,115,0>>,
         time,1562866687967115817,values,
         [0,0],
         type,
         <<105,102,95,112,97,99,107,101,116,115,0>>,
         time,1562866687967114744,values,
         [0,0],
         type,
         <<105,102,95,111,99,116,101,116,115,0>>,
         plugin_instance,
         <<118,109,110,101,116,56,0>>,
         values,
         [0,0],
         type,
         <<105,102,95,101,114,114,111,114,115,0>>,
         time,1562866687967113670,values,
         [0,0],
         type,
         <<105,102,95,112,97,99,107,101,116,115,0>>,
         time,1562866687967112596,values,
         [0,0],
         type,
         <<105,102,95,111,99,116,101,116,115,0>>,
         plugin_instance,
         <<98,114,105,100,103,101,48,0>>,
         time,1562866687967111522,values,
         [0,0],
         type,
         <<105,102,95,101,114,114,111,114,115,0>>,
         time,1562866687967110449,values,
         [0,0],
         type,
         <<105,102,95,112,97,99,107,101,116,115,0>>,
         time,1562866687967109375,values,
         [0,0],
         type,
         <<105,102,95,111,99,116,101,116,115,0>>,
         plugin_instance,
         <<98,98,112,116,112,48,0>>,
         values,
         [0,0],
         type,
         <<105,102,95,101,114,114,111,114,115,0>>,
         time,1562866687967108301,values,
         [157,0],
         type,
         <<105,102,95,112,97,99,107,101,116,115,0>>,
         time,1562866687967107227,values,
         [88774,0],
         type,
         <<105,102,95,111,99,116,101,116,115,0>>,
         plugin_instance,
         <<97,119,100,108,48,0>>,
         plugin,
         <<105,110,116,101,114,102,97,99,101,0>>,
         interval,5368709120,time,1562866687967106154,host,
         <<108,111,99,97,108,104,111,115,116,0>>]
ok
90>
90> binary_to_list(<<105,102,95,112,97,99,107,101,116,115,0>>).
[105,102,95,112,97,99,107,101,116,115,0]
91> binary_to_list(<<105,102,95,112,97,99,107,101,116,115>>).
"if_packets"
92>
92>
```
