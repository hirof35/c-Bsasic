#include<iostream>
using namespace std;

typedef struct {
	double x, y;
}Point;

int orientation(Point p,Point q,Point r) {
	double val = (q.y - p.y) * (r.x - q.x) - (q.x - p.x) * (r.y - q.y);
	if (val > 0)return 1;
	if (val < 0)return -1;
	return 0;
}
int doIntersect(Point p1, Point q1, Point p2, Point q2) {
	int o1 = orientation(p1, q1, p2);
	int o2 = orientation(p1, q1, q2);
	int o3 = orientation(p2, q2, p1);
	int o4 = orientation(p2, q2, q1);

	if (o1 != o2 && o3 != o4) return 1;
}

int main() {
	Point p1 = { 1,1 }, q1 = { 4,4 };
	Point p2 = { 1,4 }, q2 = { 4,1 };

	if (doIntersect(p1, q1, p2, q2))
		cout << "線分が交差している。" << endl;
	else
		cout << "線分が交差していない。" << endl;

	return 0;
}
