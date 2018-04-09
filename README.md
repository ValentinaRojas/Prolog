# Prolog
Base del conocimiento-Falla hepatica

:-dynamic known/3.

falla_hepatica_aguda(grado_I) :- sintoma(cambio_de_humor), sintoma(confusion), sintoma(insomnio),sintoma(falta_de_atencion).
falla_hepatica_aguda(grado_II) :- sintoma(somnoliento), sintoma(desorientado), sintoma(comportamiento_inadecuado),sintoma(inquieto).
falla_hepatica_aguda(grado_III) :- sintoma(somnolencia), sintoma(estupor), sintoma(agresividad), sintoma(hiperreflixia), sintoma(inquieto), sintoma(estuporoso), sintoma(aletargado).
falla_hepatica_aguda(grado_IV) :- sintoma(ausente), sintoma(decortificacion), sintoma(comatoso).
falla_hepatica_aguda(grado_IV_A) :- sintoma(responde_dolor).
falla_hepatica_aguda(grado_IV_B) :- sintoma(no_responde_dolor), sintoma(coma_profundo).


tratamiento(grado_I,[dieta,proteinas]).
tratamiento(grado_II,[benzoato_de_sodio,aspartato_de_ornitina,neomicina]).
tratamiento(grado_III, [metronidazol,rifaximina]).
tratamiento(grado_IV, [flumazenil,bromocriptina]).
tratamiento(grado_IV_A, [transplante_higado,antibioticos]).
tratamiento(grado_IV_B,transplante_higado).




ask(A, V):-
  known(yes, A, V),  % succeed if true
  !.  % stop looking

ask(A, V):-
  known(_, A, V),  % fail if false
  !,
  fail.

ask(A, V):-
  write(A:V),  % ask user
  write('? : '),
  nl,
  read(Y),  % get the answer
  asserta(known(Y, A, V)),  % remember it
  Y == yes.  % succeed or fail



menuask(A, V, MenuList) :-
  write('What is the value for '), write(A:V), write('?'), nl,
  write(MenuList), nl,
  read(X),
  check_val(X, A, V, MenuList),
  asserta( known(yes, A, X) ),
  X == V.

check_val(X, A, V, MenuList) :-
  member(X, MenuList),
  !.

check_val(X, A, V, MenuList) :-
  write(X), write(' is not a legal value, try again.'), nl,
  menuask(A, V, MenuList).




falla_hepatica_aguda(X) :-ask(falla_hepatica_aguda, X).
sintoma(X) :- ask(sintoma, X).

%% ----------------------------------------------------------------------------------------------
clean :- retractall(known(_,_,_)).
