#include "surakarta/surakarta_agent/surakarta_agent_mine.h"
#include <algorithm>
#include <cstdlib>
#include <random>
#include <vector>
#include "surakarta/global_random_generator.h"
#include "surakarta/surakarta_common.h"
#include "surakarta_agent_base.h"
#include "surakarta/surakarta_rule_manager.h"
#include "surakarta/surakarta_game.h"

void CalculateScore(SurakartaPosition position);
void MinusScore(SurakartaPosition position);
int BestFrom_x,BestFrom_y,BestTo_x,BestTo_y;
int LocalBestScore[7][7]={0};
int BestScore=-1000;
struct A {
    int end_x;
    int end_y;
}a[7][7];
int Score[7][7]={0};
int init[7][7]={0};
SurakartaMove SurakartaAgentMine::CalculateMove() 
{
    std::vector<SurakartaPosition> from;
    std::vector<SurakartaPosition> to;

for (unsigned int i = 0; i < board_size_; i++) 
    {
        for (unsigned int j = 0; j < board_size_; j++) 
        {LocalBestScore[i][j]=-10000;
            SurakartaPosition position = {i, j};
            if ((*board_)[i][j]->GetColor() != game_info_->current_player_&&(*board_)[i][j]->GetColor() !=PieceColor::NONE &&(*board_)[i][j]->GetColor() !=PieceColor::UNKNOWN) 
            {
         MinusScore(position); 
            } 
        }
    }
    for (unsigned int i = 0; i < board_size_; i++) 
    {
        for (unsigned int j = 0; j < board_size_; j++) 
        {
            SurakartaPosition position = {i, j};
            if ((*board_)[i][j]->GetColor() == game_info_->current_player_) 
            {
         CalculateScore(position); 
            } 
        }
    }

    
            SurakartaPosition Best_from = {BestFrom_x,BestFrom_y };
             SurakartaPosition Best_to = {BestTo_x,BestTo_y };
            


    // TODO: Implement your own ai here.
    std::cout<<"best_from:"<<Best_from.x<<","<<Best_from.y<<std::endl;
    std::cout<<"best_to:"<<Best_to.x<<","<<Best_from.y<<std::endl;
    return SurakartaMove(Best_from,Best_to, game_info_->current_player_);
}
void SurakartaAgentMine::MinusScore(SurakartaPosition position)
{
  for (unsigned int i = 0; i < 6; i++) //board_size problem
    {
        for (unsigned int j = 0; j < 6&&i!=position.x&&j!=position.y; j++) 
        {
           // specific rules
              SurakartaMove move = {(position.x,position.y),(i,j),game_info_->current_player_};
            SurakartaIllegalMoveReason reason =rule_manager_->JudgeMove(move);
    switch (reason) 
    {
        case SurakartaIllegalMoveReason::LEGAL_CAPTURE_MOVE:
            init[i][j]-=1;  // 吃子移动
        case SurakartaIllegalMoveReason::LEGAL_NON_CAPTURE_MOVE:
           // 非吃子合法移动
        case SurakartaIllegalMoveReason::NOT_PLAYER_TURN:
        case SurakartaIllegalMoveReason::OUT_OF_BOARD:
        case SurakartaIllegalMoveReason::NOT_PIECE:
        case SurakartaIllegalMoveReason::NOT_PLAYER_PIECE:
        case SurakartaIllegalMoveReason::ILLIGAL_CAPTURE_MOVE:
        case SurakartaIllegalMoveReason::ILLIGAL_NON_CAPTURE_MOVE:
        case SurakartaIllegalMoveReason::GAME_ALREADY_END:
        case SurakartaIllegalMoveReason::GAME_NOT_START:
           // 不合法移动
        case SurakartaIllegalMoveReason::LEGAL:
        case SurakartaIllegalMoveReason::ILLIGAL:
        break;
    }
    }
}
}
void SurakartaAgentMine::CalculateScore(SurakartaPosition position)//PUT IN:position.x,position.y;
{
 for(int i=0;i<6;i++)
 {
    for(int j=0;j<6;j++)
    {
        Score[i][j]=init[i][j];
    }
 }

for (unsigned int i = 0; i < 6; i++) //board_size problem
    {
        for (unsigned int j = 0; j < 6&&!(i==position.x&&j==position.y); j++) 
        {
           // specific rules
              SurakartaMove move = {(position.x,position.y),(i,j),game_info_->current_player_};
            SurakartaIllegalMoveReason reason =rule_manager_->JudgeMove(move);
    switch (reason) {
        case SurakartaIllegalMoveReason::LEGAL_CAPTURE_MOVE:
            Score[i][j]+=100;  // 吃子移动
            break;
        case SurakartaIllegalMoveReason::LEGAL_NON_CAPTURE_MOVE:
            Score[i][j]+=13;   // 非吃子合法移动
            break;
        case SurakartaIllegalMoveReason::NOT_PLAYER_TURN:
        case SurakartaIllegalMoveReason::OUT_OF_BOARD:
        case SurakartaIllegalMoveReason::NOT_PIECE:
        case SurakartaIllegalMoveReason::NOT_PLAYER_PIECE:
        case SurakartaIllegalMoveReason::ILLIGAL_CAPTURE_MOVE:
        case SurakartaIllegalMoveReason::ILLIGAL_NON_CAPTURE_MOVE:
        case SurakartaIllegalMoveReason::GAME_ALREADY_END:
        case SurakartaIllegalMoveReason::GAME_NOT_START:
          Score[i][j]+=-1000; // 不合法移动
          break;
        case SurakartaIllegalMoveReason::LEGAL:
        case SurakartaIllegalMoveReason::ILLIGAL:
        default:
          Score[i][j]+=-1000;
    }

           if(Score[i][j]>=LocalBestScore[position.x][position.y])
           {
               LocalBestScore[position.x][position.y]=Score[i][j];
               a[position.x][position.y].end_x=i;
               a[position.x][position.y].end_y=j;
           }
           if(position.x==0&&position.y==5)
           {
             std::cout<<i<<","<< j <<"->"<<";"<<Score[i][j]<<std::endl;
           }
        }
    }
    std::cout<<position.x<<","<< position.y <<"->"<< a[position.x][position.y].end_x<<","<< a[position.x][position.y].end_y<<";"<<LocalBestScore[position.x][position.y]<<std::endl;
    if(LocalBestScore[position.x][position.y]>=BestScore)
    {
      BestScore=LocalBestScore[position.x][position.y];
      BestFrom_x=position.x;
      BestFrom_y=position.y;
      BestTo_x=a[position.x][position.y].end_x;
      BestTo_y=a[position.x][position.y].end_y;
    }
}
