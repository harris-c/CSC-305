// labFive.cpp : Defines the entry point for the console application.
//

#include "stdafx.h"
#include <iostream>
#include<string>
using namespace std;

class job {
public:
	bool status; //true if allocated, false if waiting 
	int size;
	int partition;
	string name;
};

class partition {
public:
	bool free;	//true if available, false if not 
	int size;
};



int getMemSize();	//get total memory size
int getNumPart();	//get number of partitions	
void fillPartSize(partition[], int, int);	// get sizes of partitions
int fillJobList(job[]);	//get job names and sizes 
int allocate(job[], partition[], int, int);	//allocate jobs to partitions & calculate waste when jobSize < partition size
int mostWaste(int, partition[], int);	//return partition with least waste 
int getMemWaste(partition[], int, int);	//get waste when a partition is not used 
void displayWaitingJobs(job[], int);	//display jobs not allocated 

int main() {

	int totalMemSize = 0;
	int numPartitions = 0;
	partition partList[5];
	job jobList[5];
	int jobCount = 0;
	int waste = 0;

	totalMemSize = getMemSize();	//get total mem size
	numPartitions = getNumPart(); //get number of partitions
	fillPartSize(partList, numPartitions, totalMemSize);	//get sizes of partitions
	cout << endl << "Fill the job list" << endl;
	jobCount = fillJobList(jobList);	// get names and sizes of jobs 
	cout << endl << "Initial memory allocation: " << endl;
	waste = allocate(jobList, partList, jobCount, numPartitions);
	waste = getMemWaste(partList, numPartitions, waste);
	cout << "The total memory waste is: " << waste << endl;
	displayWaitingJobs(jobList, jobCount);
}

int getMemSize() {
	int x;
	cout << "Enter the total size of the memory: ";
	cin >> x;
	return x;
}

int getNumPart() {
	int x;
	cout << "Enter the number of partitions: ";
	cin >> x;
	if (x >5) {	//limit of 5 partitions 
		x = 5;
	}
	return x;
}

void fillPartSize(partition list[], int x, int y) {
	for (int i = 0; i<x; i++) {	//loop to get size of partition
		if (y <= 0) {
			list[i].size = 0;
			cout << "There is no memory left. Partition " << i + 1 << " has size 0" << endl;
		}
		else {
			cout << "Enter the size of partition " << i + 1 << " : ";	//index is partion number 
			cin >> list[i].size;	//[index] is size
		}
		list[i].free = true;
		y = y - list[i].size;	//calculate memoy left 
		cout << "There is " << y << " memory room available" << endl;
	}
}

int fillJobList(job list[]) {
	char addJobs = 'y';
	int jobCount = 0;

	cout << "Enter the total number of jobs: ";
	cin >> jobCount;

	for (int i = 0; i<jobCount; i++) {
		cout << "Enter the name of the job: ";	//get name of job
		cin >> list[i].name;
		cout << "Enter the size of the job: ";	//get size of job 
		cin >> list[i].size;
	}

	return jobCount;
}

int allocate(job list[], partition part[], int numJobs, int numPart) {	//list is job list, part is partitions
	int waste = 0;	// this is only waste where job size < partition size

	for (int i = 0; i<numJobs; i++) {
		int pos = mostWaste(list[i].size, part, numPart);

		if (pos >= 0) {
			cout << list[i].name << " has been allocated in partition " << pos + 1 << endl;	//allocate job	
			list[i].status = true;	//job has been allocated
			list[i].partition = pos;	//partition number 
			part[pos].free = false;	//partition is filled 
			waste += part[pos].size - list[i].size;	//calculate waste
		}
		else {
			list[i].status = false;	//job has not been allocated
			part[pos].free = true;	//partition is not filled 
		}
	}
	return waste;
}
int getMemWaste(partition parts[], int numParts, int tempWaste) {

	for (int i = 0; i<numParts; i++) {
		if (parts[i].free == true) {
			tempWaste += parts[i].size;	//if partition is empty, then waste
		}
	}
	return tempWaste;
}

void displayWaitingJobs(job list[], int numJobs) {
	for (int i = 0; i<numJobs; i++) {
		if (list[i].status == false) {	//if job has not been allocated
			cout << "Job " << list[i].name << " has not been allocated and is waiting" << endl;
		}
	}
}

int mostWaste(int jobSize, partition part[], int numP) {

	int temp = -1;
	int pos = -1;
	int temp2 = -1;

	//this loops finds the first available partition to fit the job 
	for (int i = 0; i<numP; i++) {
		if (jobSize <= part[i].size && part[i].free == true) {			 //job will fit and partition is available 
			temp = part[i].size - jobSize;
			pos = i;
			break;
		}
	}
	//this loop goes through all possible partitions and finds the one with the smallest memory waste
	for (int j = pos; j<numP; j++) {
		if (jobSize <= part[j].size && part[j].free == true) {
			temp2 = part[j].size - jobSize;
		}
		if (temp2 > temp) {
			temp = temp2;
			pos = j;
		}
	}
	return pos;

}
