# args

```
# コマンド名を決めてない。適当
mylitega 
```

# define_func (objective function and constraint)
knapsack example
https://www.geeksforgeeks.org/extended-knapsack-problem/
Input: N = 5, P[] = {2, 7, 1, 5, 3}, C[] = {2, 5, 2, 3, 4}, W = 8, K = 2.
```
# $1 is a solution that is array of how many items should put in knapsack: x=( 0,1,0,1,0 )
define_func () {
  arr_x=$1

  # contraint
  mat_v_w=( "2 2" "7 5" "1 2" "5 3" "3 4" )
  w_max=8
  k_max=2
  ## too many items
  k_sum=0
  for k in "${arr_x[@]}" ; do
    k_sum=$k_sum+$k
  done
  if [ k_sum -gt k_max ] ; then
    return 0
  fi

  # objective function
  v_sum=0
  w_sum=0
  for i in "${mat_v_w[@]}" ; do
    item=(${i[@]})
    value=${item[0]}
    ## too much weight (constraint)  <- splitting mat_v_w and calc-ing this in constraint section is better
    w_sum=w_sum+${item[1]}
    if [ w_sum -gt w_max ] ; then
      return 0
    fi
    ## value
    v_sum=v_sum+value
  done
  ## to change maximize into minimize
  return -v_sum
}
```
