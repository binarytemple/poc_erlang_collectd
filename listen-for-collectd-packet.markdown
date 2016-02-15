```
{ok, Socket} = gen_udp:open(25826, [binary, {active, false}]),
io:format("server opened socket:~p~n",[Socket]),
inet:setopts(Socket, [{active, once}]).

{udp, Packet} = receive
    {udp, Socket, Host, Port, Bin} ->
      io:format("server received:~p~n", [Bin]),
    {udp, Bin}   
      ;
    Other ->
      io:format("server received:~p~n", [Other]),
      Other
  after 80000 -> 0
  end.


<<Type:16/big-unsigned-integer,LengthWithHeader:16/big-unsigned-integer,Rest/binary>> = Bin.
```
