我现在跟着一个YouTube视频（ https://www.youtube.com/watch?v=wuSQpLinRB4 ），学习复刻AlphaZero的算法。这个视频中的代码已经同时开源到GitHub上了，其链接为：[text](https://github.com/foersterrobert/AlphaZeroFromScratch)。
我对于这个项目的代码有很多不理解的地方，我希望你能根据我后续发给你的代码，给我详细地解释。
首先，我先将第一部分的代码发给你，这部分的代码主要实现了tictactoe的基础逻辑，并且实现了玩家和玩家对战的功能：

```python
import numpy as np

class TicTacToe:
    def __init__(self):
        self.row_count = 3
        self.column_count = 3
        self.action_size = self.row_count * self.column_count

    def get_initial_state(self):
        return np.zeros((self.row_count, self.column_count))

    def get_next_state(self, state, action, player):
        row = action // self.column_count
        column = action % self.column_count
        state[row,column]=player
        return state

    def get_valid_moves(self, state):
        return (state.reshape(-1) == 0).astype(np.uint8)
    
    def check_win(self, state, action):
        if action == None:
            return False
        row=action //self.column_count
        column=action %self.column_count
        player=state[row,column]
        return (
            np.sum(state[row,:])==player*self.column_count
            or np.sum(state[:,column])==player*self.row_count
            or np.sum(np.diag(state))==player*self.row_count
            or np.sum(np.diag(np.flip(state,axis=0)))==player*self.row_count
        )
    
    def get_value_and_terminated(self, state, action):
        if self.check_win(state, action):
            return 1, True
        if np.sum(self.get_valid_moves(state))==0:
            return 0, True
        return 0, False
    
    def get_opponent(self, player):
        return -player
    
tictactoe=TicTacToe()
player=1

state=tictactoe.get_initial_state()

while True:
    print(state)
    valid_moves=tictactoe.get_valid_moves(state)
    print("Valid moves:",[i for i in range(tictactoe.action_size) if valid_moves[i]==1])
    action=int(input(f"{player}: "))

    if valid_moves[action]==0:
        print("Invalid move")
        continue

    state=tictactoe.get_next_state(state, action, player)

    value,is_terminated=tictactoe.get_value_and_terminated(state, action)

    if is_terminated:
        print(state)
        if value==1:
            print(f"Player {player} wins")
        else:
            print("Draw")
        break

    player=tictactoe.get_opponent(player)
```

请你对这个代码的全同逻辑进行梳理，然后我会提出我没有搞懂的部分，请你再给我解释。等我完全弄清楚这段代码后，我再继续给你发接下来我希望理解的代码。