// labNine.cpp : Defines the entry point for the console application.
//

#include "stdafx.h"
#include<iostream>
using namespace std;

class process {
public:
	int arrivalTime;
	int CPUTime;
	int termTime;
	bool started = false;
	int startTime;
	int priority;
};

int getNumProcesses();
void getArrivalTime(process *, int);
void getCPUTime(process *, int);
void getPriority(process*, int);
void displayStartTime(process, int, int);
void displayTermTime(process, int, int);
int getTotalTime(process*, int);
bool isDone(process*, int);
void timePass(process*, int);
int getIndividTurnaround(process*, int);
int getAvailableJob(process*, int, int);
void turnaroundTime(process*, int);



int main()
{
	int numProcesses = getNumProcesses(); //number of processes 
	process* processList = new process[numProcesses]; //array to hold processes 
	bool first = true;		//if job is first schedueled, starts at current time
	bool needJob = true;	//used to see if a new job needs to be found(get a new index)
	int timeNow = 0;
	int index = 0;  //keep track of current job
	int prevIndex = 0;

	getArrivalTime(processList, numProcesses);
	getPriority(processList, numProcesses);
	getCPUTime(processList, numProcesses);

	int totalTime = getTotalTime(processList, numProcesses); //returns total time for all processes to terminate
															 //loop will run until all jobs have terminated


	for (timeNow; timeNow <= totalTime; timeNow++) {
		//if job has not been started, start it
		//there are two ways to go: either this is the first job and starts at time 0
		//or it starts at the time of the preceding process's termination time

		//get index of next job
		if (needJob == true) {
			index = getAvailableJob(processList, timeNow, numProcesses);
			needJob = false;
		}
		if (processList[index].started == false) {
			processList[index].started = true;
			if (first == true) {
				processList[index].startTime = timeNow;
			}
			else {
				timeNow--;
				processList[index].startTime = processList[prevIndex].termTime;
			}
			first = false;
			displayStartTime(processList[index], index, timeNow);
		}

		//if process has terminated 
		if (isDone(processList, index)) {
			processList[index].termTime = timeNow;
			displayTermTime(processList[index], index, timeNow);
			prevIndex = index;
			needJob = true;
		}
		else {
			timePass(processList, index);
		}
	}
	turnaroundTime(processList, numProcesses);
	
	return 0;
}
//get total number of processes 
int getNumProcesses() {
	int x;
	cout << "Enter the number of processes to be scheduled: ";
	cin >> x;
	return x;
}
//get the arrival times for all processes 
void getArrivalTime(process* list, int numP) {
	for (int i = 0; i < numP; i++) {
		cout << "Enter the arrival time for process P" << i + 1 << " ";
		cin >> list[i].arrivalTime;
	}
	cout << endl;
}
//get CPU time for all processes
void getCPUTime(process * list, int numP) {
	for (int i = 0; i < numP; i++) {
		cout << "Enter the CPU time for process P" << i + 1 << " ";
		cin >> list[i].CPUTime;
	}
	cout << endl;
}
//get priority for processes
void getPriority(process * list, int numP) {
	for (int i = 0; i < numP; i++) {
		cout << "Enter the priority for process P" << i + 1 << " ";
		cin >> list[i].priority;
	}
	cout << endl;
}

void displayStartTime(process p, int num, int time) {
	cout << "Process P" << num + 1 << " with priority " << p.priority << " started at time " << time << endl;
}

void displayTermTime(process p, int num, int time) {
	cout << "Process P" << num + 1 << " terminated at time " << time << endl << endl;
}
// total time is sum of individual CPU time
int getTotalTime(process * list, int numP) {
	int sum = 0;
	for (int i = 0; i < numP; i++) {
		sum += list[i].CPUTime;
	}
	return sum;
}
//process is done when CPU time = 0
bool isDone(process* list, int index) {
	if (list[index].CPUTime == 0) {
		return true;
	}
	return false;
}
//decrement one from CPU time
void timePass(process* list, int index) {
	list[index].CPUTime--;
}
//return turnaround time for one process
int getIndividTurnaround(process * list, int index) {
	return list[index].termTime - list[index].arrivalTime;
}
//return index of job to be schedules next 
int getAvailableJob(process * list, int time, int numProcess)
{
	int temp = -1;
	int temp1 = -1;
	//this loop gets the first job available 
	for (int i = 0; i < numProcess; i++) {
		if (list[i].started == false && list[i].arrivalTime <= time) {
			temp = i;
			break;
		}
	}
	//this loop finds the highest priority job of available jobs 
	for (int i = 0; i < numProcess; i++) {
		if (list[i].started == false && list[i].arrivalTime <= time) {
			temp1 = i;
		}
		if (temp1 >= 0 && list[temp1].priority < list[temp].priority) {
			temp = temp1;
		}
	}

	return temp;
}
void turnaroundTime(process* list, int n) {
	double sum = 0;
	//output individual turnaround times for each process and add that to a sum variable
	for (int i = 0; i < n; i++) {
		int tIndiv = getIndividTurnaround(list, i);
		cout << "Process P" << i + 1 << " had turnaround time " << tIndiv << endl;
		sum += tIndiv;
	}
	//total turnaround is sum of individuals / total number
	cout << "The total turnaround time is " << sum / n << endl << endl;
}
