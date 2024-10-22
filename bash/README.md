とりあえず、引数に複雑な関数（問題設定と目的関数と制約条件が入ってる）
# args

```
# コマンド名を決めてない。適当
mylitega 
```

# define_func (problem define, objective function and constraint)
knapsack example  
https://www.geeksforgeeks.org/extended-knapsack-problem/  
Input: N = 5, P[] = {2, 7, 1, 5, 3}, C[] = {2, 5, 2, 3, 4}, W = 8, K = 2.
```
# $1 is a solution that is array of how many items should put in knapsack: x=( 0,1,0,1,0 )
define_func () {
  # solution(=input) and global return variable
  local arr_x=($1)
  MYLITEGA_RETU_DEFI_FUNC=99999
  echo INPUT====$1
  #echo DBG____${arr_x[@]}
  # problem define
  local mat_v_w=( "2 2" "7 5" "1 2" "5 3" "3 4" )
  local w_max=8
  local k_max=2
  # TODO: arg error
  # contraint
  ## too many items
  local k_sum=0
  for k in "${arr_x[@]}" ; do
    k_sum=$((k_sum + k))
  done
  if [ $k_sum -gt $k_max ] ; then
    echo over_k_OUTPUT====$MYLITEGA_RETU_DEFI_FUNC
    return 1
  fi

  # objective function
  local v_sum=0
  local w_sum=0
  local k
  local item
  local value
  local weight
  for ((i=0; ${#mat_v_w[@]}>$i; i++)) ; do
    k=${arr_x[i]}
    item=(${mat_v_w[i]})
    value=${item[0]}
    weight=${item[1]}
    ## too much weight (constraint)  <- splitting mat_v_w and calc-ing this in constraint section is better
    w_sum=$(( w_sum + weight * k ))
    if [ $w_sum -gt $w_max ] ; then
      echo over_weight_OUTPUT====$MYLITEGA_RETU_DEFI_FUNC
      return 2
    fi
    ## value
    v_sum=$((v_sum + value *k))
  done
  ## to change maximize into minimize
  MYLITEGA_RETU_DEFI_FUNC=$((-v_sum))
  echo OUTPUT====$MYLITEGA_RETU_DEFI_FUNC
  return 0
}
# e.g.:
# myans=(0 0 1 0 0)
# define_func "${myans[*]}"
# echo $MYLITEGA_RETU_DEFI_FUNC
```
