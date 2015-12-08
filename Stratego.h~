#pragma once

#include <iostream>
#include <vector>
#include <string>
#include <queue>
#include <list>
#include <algorithm>
#include <stdlib.h>
//#include "Grid.h"
//#include "Actor.h"
//#include "Tree.h"
//#include "Tree2.h"
//#include <iterator>

using namespace std;

//class Board;

#define SEARCH_DEPTH 5
//#define MAX_DEPTH 5	

struct Move
{
	Move() { }
	Move(int a, int b, int c, int d) { x1 = a; y1 = b; x2 = c; y2 = d; }

	int x1, y1, x2, y2;
};

class Stratego
{
public:
	Stratego();
	vector <string> Initialize();
	void Play(int & x1, int & y1, int & x2, int & y2);
	void Capture(char piece);
	void Update(int x1, int y1, int x2, int y2, char piece);

private:
	int x1, y1, x2, y2;
	int team;
	int depth;
	//int moveList[MAX_DEPTH][4];//the movelist stores moves for every state we traverse through
	char mycolor;
	vector<string> color;
	vector<string> board;
	int getTeam();
	//vector<string> scores;
	bool CanMove(int, int, int, int);
	int * traversetree();
	Move search(int team, int depth, int & val, int alpha, int beta);
	//int Move(int, int, int, int);
	int point();
	//void moveStore(int, int, int, int);
	int Evaluation(int, int, int, int);
	bool inDanger(int, int, vector<string>, int);
	int threat(int, int, vector<string>, int);

};

Stratego::Stratego()
{
	mycolor = 'R';
	color.push_back("BBBBBBBBBB");
	color.push_back("BBBBBBBBBB");
	color.push_back("BBBBBBBBBB");
	color.push_back("BBBBBBBBBB");

	color.push_back("EEEEEEEEEE");
	color.push_back("EEEEEEEEEE");

	color.push_back("RRRRRRRRRR");
	color.push_back("RRRRRRRRRR");
	color.push_back("RRRRRRRRRR");
	color.push_back("RRRRRRRRRR");

	board.push_back("??????????");
	board.push_back("??????????");
	board.push_back("??????????");
	board.push_back("??????????");

	board.push_back("00RR00RR00");
	board.push_back("00RR00RR00");

	board.push_back("9B7799S7BB");
	board.push_back("3566215434");
	board.push_back("997995B865");
	board.push_back("86898BFB48");
}

vector <string> Stratego::Initialize()
{
	vector <string> init;
	init.push_back("86898BFB48");
	init.push_back("997995B865");
	init.push_back("3566215434");
	init.push_back("9B7799S7BB");
	return init;
}

// moves the actor at the specified location
// to the other specified location
// returns 1 if the move was successful
// 0 otherwise

int Stratego::getTeam()
{

	for (int i = 0; i<10; i++)
	for (int j = 0; j<10; j++)
	{
		if (color[i][j] == 'R')
			team = 1;
		if (color[i][j] == 'B')
			team = 0;
	}
	return team;
}
int Stratego::point()
{
	int pieceTotal = 0;
	int distTotal = 0;
	int num = 0;
	int te;
	int numEnemyPiecesRemaining = 0;
	for (int m = 0; m < 10; m++)
	{
		for (int n = 0; n < 10; n++)
		{
			if (color[m][n] = 'B') ++numEnemyPiecesRemaining;
		}
	}
	for (int i = 0; i < 10; i++)
	{
		for (int j = 0; j < 10; j++)
		{
			if (board[i][j])
			{
				int ty = atoi(&board[i][j]);
				if (color[i][j] == 'R') te = 1;
				else if (color[i][j] == 'B') te = 0;
				if (te == 0 && numEnemyPiecesRemaining > 15)
					distTotal += 20 - (i + j);
				else if (te == 0 && board[i][j] == '8'/*ty == 8*/)
					distTotal += (20 - (i + j)) * 45;
				if (te == 1)
				{
					switch (ty)
					{
					case 66://Bomb
						pieceTotal += 100;
						break;
					case 70://Flag
						pieceTotal += 1000000;
						break;
					case 56://Miner
						pieceTotal += 50;
					default:
						if (board[i][j] == 'S')ty = 10;
						pieceTotal += 50 - ty;
						break;
					}
				}
			}
		}
	}
	return 5 * pieceTotal + distTotal;
}
Move Stratego::search(int team, int depth, int & val, int alpha, int beta)
{

	// these vectors store all possible moves and their corresponding values
	vector<int> values;
	vector<Move> moves;
	vector<string> state;
	//state[0][0] = board[9][0];
	for (int i = 0; i < 10; i++)
	{
		state.push_back(board[i]);
	}
	//Grid* state;
	// if we've reached our max depth we return the value of the state and NULL for the move
	if (depth == SEARCH_DEPTH + 1)
	{
		val = point();
		return Move(0, 0, 0, 0);
	}

	// iterate through entire board to consider every possible move
	for (int x1 = 10; x1 >= 0; x1++)
	{
		for (int y1 = 10; y1 >= 0; y1++)
		{
			// check if there's an actor at the spot
			if (board[x1][y1])
			{
				// and make sure it's on the right team
				if (color[x1][y1] == 'R'/*state[x1][y1]*/)
				{
					// now we check every move this piece could make,
					// and add each move to our moves vector
					// if it's a scout we must consider its movement
					if (board[x1][y1] == '9')
					{
						for (int x2 = 0; x2 < 10; x2++)
						if (CanMove(x1, y1, x2, y1))
						{

							moves.push_back(Move(x1, y1, x2, y1));
							values.push_back(Evaluation(x1, y1, x2, y1));
						}
						for (int y2 = 0; y2 < 10; y2++)
						if (CanMove(x1, y1, x1, y2))
						{
							moves.push_back(Move(x1, y1, x1, y2));
							values.push_back(Evaluation(x1, y1, x1, y2));
						}
					}
					else
					{
						if (CanMove(x1, y1, x1 - 1, y1))
						{
							moves.push_back(Move(x1, y1, x1 - 1, y1));
							values.push_back(Evaluation(x1, y1, x1 - 1, y1));
						}
						if (CanMove(x1, y1, x1 + 1, y1))
						{
							moves.push_back(Move(x1, y1, x1 + 1, y1));
							values.push_back(Evaluation(x1, y1, x1 + 1, y1));
						}
						if (CanMove(x1, y1, x1, y1 - 1))
						{
							moves.push_back(Move(x1, y1, x1, y1 - 1));
							values.push_back(Evaluation(x1, y1, x1, y1 - 1));
						}
						if (CanMove(x1, y1, x1, y1 + 1))
						{
							moves.push_back(Move(x1, y1, x1, y1 + 1));
							values.push_back(Evaluation(x1, y1, x1, y1 + 1));
						}
					}
				}
			}
		}
	}
	// now we check which of the possible moves has the highest (or lowest, depending on which team is making the move)
	// value
	int bestVal = 0;
	int bestIndex = 0;
	// make sure there is at least one valid move
	if (moves.size() > 0)
	{
		/*
		// first we check the first move and compare the rest to it
		state->move(moves[0]->x1, moves[0]->y1, moves[0]->x2, moves[0]->y2, team);
		search(1 - team, depth + 1, bestVal);
		state->undoMove();
		for (int i = 1; i < moves.size(); i++)
		{
		// alter the state by making the move
		state->move(moves[i]->x1, moves[i]->y1, moves[i]->x2, moves[i]->y2, team);
		int v;
		// check the value of this state as determined by the max/min of the states that can occur after it
		search(1 - team, depth + 1, v);
		// if it's the computer we want to maximize value
		if (team == player->getTeam())
		{
		if (v > bestVal)
		{
		bestIndex = i;
		bestVal = v;
		}
		}
		// otherwise minimize it
		else
		{
		if (v < bestVal)
		{
		bestIndex = i;
		bestVal = v;
		}
		}
		state->undoMove();
		}
		*/
		int aindex = 0, bindex = 0;
		for (int i = 0; i < moves.size(); i++)
		{
			// alter the state by making the move
			//state->move(moves[i].x1, moves[i].y1, moves[i].x2, moves[i].y2, team);
			int v = 0;
			// check the value of this state as determined by the max/min of the states that can occur after it
			search(1 - team, depth + 1, v, alpha, beta);
			// if it's the computer we want to maximize value
			//state->undoMove();

			if (team == getTeam())
			{
				if (v > alpha)
				{
					alpha = v;
					aindex = i;
				}
				if (alpha >= beta)
				{
					val = alpha;
					return moves[i];
				}
			}
			// otherwise minimize it
			else
			{
				if (v < beta)
				{
					beta = v;
					bindex = i;
				}
				if (alpha >= beta)
				{
					val = beta;
					return moves[i];
				}
			}
		}
		if (team == getTeam())
		{
			val = alpha;
			return moves[aindex];
		}
		else
		{
			val = beta;
			return moves[bindex];
		}
		/*
		val = bestVal;
		// clean up, but if we're at a depth of 1, we're returning the move to the user
		// so don't delete that one.
		if (depth > 1)
		{
		for (int i = 0; i < moves.size(); i++) delete moves[i];
		return NULL;
		}
		else
		{
		for (int i = 0; i < moves.size(); i++) { if (bestIndex != i) delete moves[i]; }
		return moves[bestIndex];
		}
		*/
	}
	else
	{
		val = 0;
		return Move(0, 0, 0, 0);
	}
}

int Stratego::Evaluation(int x1, int y1, int x2, int y2)
{

	//pair<char, int>points;

	//determines where our flag is so that we can account for threat if piece is on our side
	int ourflagY = 0;
	for (int i = 0; i < 10; i++)
	{
		for (int j = 0; j < 10; j++)
		{
			if (board[i][j] && board[i][j] == 'F' && color[i][j] == 'R')
			{
				ourflagY = j;
			}
		}
	}
	int flagX = 0;
	int flagY = 0;
	//search board for where their flag is
	for (int i = 0; i < 10; i++)
	{
		for (int j = 0; j < 10; j++)
		{
			if (board[i][j] && board[i][j] == 'F' && color[i][j] == 'B')
			{
				flagX = i;
				flagY = j;
			}
		}
	}
	//need to make sure math is included so we can use absolute value
	int distToFlag = abs(x1 - flagX) + abs(y1 - flagY);
	int newdistToFlag = abs(x2 - flagX) + abs(y2 - flagY);
	//number of their actors remaining to decide how important certain moves are
	int actorsRemaining = 0;
	for (int i = 0; i < 10; i++)
	{
		for (int j = 0; j < 10; j++)
		{
			if (board[i][j] && color[i][j] == 'B')
			{
				actorsRemaining++;
			}
		}
	}
	int worth = 0;
	int val = atoi(&board[x1][y1]);
	if (board[x1][y1] == 'B') val = 0;
	if (board[x1][y1] == 'F') val = 11;
	if (board[x1][y1] == 'S') val = 10;
	bool startdanger = inDanger(x1, y1, color, val);
	bool movedanger = inDanger(x2, y2, color, val);
	if (movedanger)
		worth -= 10 - val;
	if (startdanger == true && movedanger == false)
		worth += 10 - val;
	if (board[x2][y2])
	{
		//checks moving into bomb
		if (board[x2][y2] == 'B')
		{
			if (val == 8)
			{
				//add five because it is good to take out a bomb, even if it puts the minor in danger (5-2 = 3 so move is positive)
				worth += 5;
			}
			else
				worth -= 10 - val;
		}
		//checks moving 1 into spy, one of the worst possible moves
		if (board[x2][y2] == 'S' && val == 1)
			worth -= 11;
		//adds attacking value
		if (val < atoi(&board[x2][y2]))
			worth += 10 - atoi(&board[x2][y2]) + (threat(x2, y2, color, ourflagY) / 2);
		//subtracts dying value and makes sure that we aren't moving into a bomb to avoid double counting
		if (val> atoi(&board[x2][y2]) && board[x2][y2] != 'B')
			worth -= 10 - val;
		//takes flag if can
		if (board[x2][y2] == 'F')
			worth += 1000;
	}
	//moving closer to the flag is somewhat beneficial
	if (newdistToFlag<distToFlag)
		worth += 1;
	//moving a miner to the flag is better than most other pieces because it can take out bombs
	if (newdistToFlag<distToFlag && val == 8 && actorsRemaining<35)
		worth += 2;
	//early game scenarios its better to advance good pieces
	if (newdistToFlag<distToFlag && val <= 4)
		worth += 1;

	return worth;
}

bool Stratego::inDanger(int x, int y, vector<string> colour, int val)
{
	//check up, down, left and right pieces; makes sure actor exists . 
	int val1 = atoi(&board[x - 1][y]);
	if (board[x - 1][y] == 'B') val1 = 0;
	if (board[x - 1][y] == 'F') val1 = 11;
	if (board[x - 1][y] == 'S') val1 = 10;
	if (x > 0 && board[x - 1][y] &&
		/*state->getActor(x - 1, y)->getTeam() != team*/colour[x - 1][y] != color[x][y] && val1 <= val && board[x - 1][y] != 'B')
		return true;
	val1 = atoi(&board[x + 1][y]);
	if (board[x + 1][y] == 'B') val1 = 0;
	if (board[x + 1][y] == 'F') val1 = 11;
	if (board[x + 1][y] == 'S') val1 = 10;
	if (x < 9 && board[x + 1][y] &&
		/*state->getActor(x + 1, y)->getTeam() != team*/ colour[x + 1][y] != color[x][y] && /*state->getActor(x + 1, y)->getType()*/val1 <= val && /*state->getActor(x + 1, y)->getType()>0*/board[x + 1][y] != 'B')
		return true;
	val1 = atoi(&board[x][y - 1]);
	if (board[x][y - 1] == 'B') val1 = 0;
	if (board[x][y - 1] == 'F') val1 = 11;
	if (board[x][y - 1] == 'S') val1 = 10;
	if (y > 0 && board[x][y - 1] &&
		colour[x][y - 1] != color[x][y - 1] && val1 <= val && board[x][y - 1] != 'B')
		return true;
	val1 = atoi(&board[x][y + 1]);
	if (board[x][y + 1] == 'B') val1 = 0;
	if (board[x][y + 1] == 'F') val1 = 11;
	if (board[x][y + 1] == 'S') val1 = 10;
	if (y < 9 && board[x][y + 1] &&
		colour[x][y + 1] != color[x][y + 1] && val1 <= val && board[x][y + 1] != 'B')
		return true;
	return false;
}

//checks what pieces the oponent can kill aka what pieces we would be saving by killing them .
int Stratego::threat(int x, int y, vector<string> colour, int flagplace)
{
	//check up, down, left and right pieces to see what it can kill .
	int val = atoi(&board[x][y]), t = 0;
	if (board[x][y] == 'B') val = 0;
	if (board[x][y] == 'F') val = 11;
	if (board[x][y] == 'S') val = 10;

	int val1 = atoi(&board[x - 1][y]);
	if (board[x - 1][y] == 'B') val1 = 0;
	if (board[x - 1][y] == 'F') val1 = 11;
	if (board[x - 1][y] == 'S') val1 = 10;

	if (x > 0 && board[x - 1][y] && colour[x - 1][y] == color[x - 1][y] && val1 >= val && board[x - 1][y] != 'B')
		t += 10 - /*state->getActor(x - 1, y)->getType();*/val1;
	val1 = atoi(&board[x - 1][y]);
	if (board[x + 1][y] == 'B') val1 = 0;
	if (board[x + 1][y] == 'F') val1 = 11;
	if (board[x + 1][y] == 'S') val1 = 10;
	if (x < 9 && board[x + 1][y] && colour[x + 1][y] == color[x + 1][y] && val1 >= val && board[x + 1][y] != 'B')
		t += 10 - val1;
	val1 = atoi(&board[x][y - 1]);
	if (board[x][y - 1] == 'B') val1 = 0;
	if (board[x][y - 1] == 'F') val1 = 11;
	if (board[x][y - 1] == 'S') val1 = 10;
	if (y > 0 && board[x][y - 1] && colour[x][y - 1] == color[x][y - 1] && val1 >= val && board[x][y - 1] != 'B')
		t += 10 - val1;
	val1 = atoi(&board[x][y + 1]);
	if (board[x][y + 1] == 'B') val1 = 0;
	if (board[x][y + 1] == 'F') val1 = 11;
	if (board[x][y + 1] == 'S') val1 = 10;
	if (y < 9 && board[x][y + 1] && colour[x][y + 1] == color[x][y + 1] && val1 >= val && board[x][y + 1] != 'B')
		t += 10 - val1;
	//adds to the threat if they are on our side .
	if (flagplace <= 3 && y <= 4)
	{
		t += 2;
	}
	if (flagplace >= 6 && y >= 5)
	{
		t += 2;
	}
	if (flagplace == y)
	{
		t += 2;
	}
	return t;
}

void Stratego::Play(int & x1, int & y1, int & x2, int & y2)
{
	//vector<pair<int, int>> source;
	//vector< pair<int, int> >::iterator it;
	//vector<pair<int, int>> destination;
	//pair<pair<int, int>, int>bestMove;
	Move m;

	int v = point();
	m = search(team, 1, v, -100000000, 100000000);
	x1 = m.x1;
	y1 = m.y1;
	x2 = m.x2;
	y2 = m.y2;
}

bool Stratego::CanMove(int x1, int y1, int x2, int y2)
{
	if (x1 >= 10 || y1 >= 10 || x2 >= 10 || y2 >= 10 || x1 < 0 || y1 < 0 || x2 < 0 || y2 < 0)
		return false;
	if (x2 != x1 && y2 != y1)//checks to see if move is diagonal
		return false;
	if (board[x1][y1] == NULL)//makes sure there is an actor in the spot we are trying to move
		return false;
	if (board[x1][y1] == 'F')
		return false;
	if (board[x1][y1] == 'B')
		return false;
	if (x1 == x2 && y1 == y2)
		return false;
	if (color[x1][y1] == color[x2][y2])
		return false;
	if (board[x2][y2] == 'R')
		return false;

	if (board[x1][y1] >= '1' && board[x1][y1] <= '9' || board[x1][y1] == 'S')//checks if its a piece that can move (not bomb or lake or flag) 
	{
		if (board[x1][y1] == '9')
		{
			if (x1 == x2)
			{
				int temp = y1;
				if (y2 - y1 == 1 || y2 - y1 == -1) //returns true if it's moving one before going through the other checks because the other checks would return false because there is something at y2 
					return true;
				if (y2>y1)
				{
					//make sure that we don't check the (x1, y1) 
					temp++;
					while (temp <= y2) //checks that everything in between is null 
					{
						if (board[x1][temp])
						{
							if (temp == y2)
								return true;
							else
								return false;
						}
						temp++;
					}
					return true;
				}
				else
				{
					temp--;
					while (temp >= y2) //checks that everything in between is null 
					{
						if (board[x1][temp])
						{
							if (temp == y2)
								return true;
							else
								return false;
						}
						temp--;
					}
					return true;
				}
			}
			if (y1 == y2)
			{
				int temp = x1;
				if (x2 - x1 == 1 || x2 - x1 == -1)//returns true if it's moving one before going through the other checks because the other checks would return false because there is something at y2 
					return true;
				if (x2>x1)
				{
					temp++;
					while (temp <= x2)//checks that everything in between is null 
					{
						if (board[temp][y1])
						{
							if (temp == x2)
								return true;
							else
								return false;
						}
						temp++;
					}
					return true;
				}
				else
				{
					temp--;
					while (temp >= x2)//checks that everything in between is null 
					{
						if (board[temp][y1])
						{
							if (temp == x2)
								return true;
							else
								return false;
						}
						temp--;
					}
					return true;
				}
			}
		}
		else
		{
			if (x2 - x1 == 1 || x2 - x1 == -1 || y2 - y1 == 1 || y2 - y1 == -1)
				return true;
			else
				return false;
		}
	}
}

void Stratego::Capture(char piece)
{
	char mp = board[x1][y1];

	board[x1][y1] = '0';
	color[x1][y1] = 'E';

	if (piece == mp)
	{
		board[x2][y2] = '0';
		color[x2][y2] = 'E';
	}
	else if (piece == 'B')
	{
		if (mp == '8')
		{
			board[x2][y2] = mp;
			color[x2][y2] = mycolor;
		}
	}
	else if (mp == 'S' && piece == '1')
	{
		board[x2][y2] = mp;
		color[x2][y2] = mycolor;
	}
	else if (piece == 'S' || piece == 'F')
	{
		board[x2][y2] = mp;
		color[x2][y2] = mycolor;
	}
	else if (mp < piece || piece == '0')
	{
		board[x2][y2] = mp;
		color[x2][y2] = mycolor;
	}
}

void Stratego::Update(int x1, int y1, int x2, int y2, char piece)
{
	--x1;
	--y1;
	--x2;
	--y2;
	if (piece == '0')
	{
		board[x2][y2] = board[x1][y1];
		color[x2][y2] = color[x1][y1];
	}
	else if (board[x2][y2] == piece)
	{
		board[x2][y2] = '0';
		color[x2][y2] = 'E';
	}
	else if (board[x2][y2] == 'B')
	{
		if (piece == '8')
		{
			board[x2][y2] = piece;
			color[x2][y2] = color[x1][y1];
		}
	}
	else if (piece == 'S' && board[x2][y2] == '1')
	{
		board[x2][y2] = piece;
		color[x2][y2] = color[x1][y1];
	}
	else if (board[x2][y2] == 'S' || board[x2][y2] == 'F')
	{
		board[x2][y2] = piece;
		color[x2][y2] = color[x1][y1];
	}
	else if (piece < board[x2][y2])
	{
		board[x2][y2] = piece;
		color[x2][y2] = color[x1][y1];
	}

	board[x1][y1] = '0';
	color[x1][y1] = 'E';
}
