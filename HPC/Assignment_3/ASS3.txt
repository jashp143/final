#include <iostream>
#include <vector>
#include <cstdlib>
#include <ctime>
#include <climits>
#include <omp.h>
using namespace std;

void minimum(vector<int>& array) {
    int min_val = INT_MAX;
    double start = omp_get_wtime();
    for (int i = 0; i < array.size(); i++) {
        if (array[i] < min_val) min_val = array[i];
    }
    double end = omp_get_wtime();
    cout << "Minimum Element: " << min_val << endl;
    cout << "Time Taken: " << (end - start) << " seconds" << endl;

    int min_ele = INT_MAX;
    start = omp_get_wtime();
    #pragma omp parallel for reduction(min:min_ele)
    for (int i = 0; i < array.size(); i++) {
        if (array[i] < min_ele) min_ele = array[i];
    }
    end = omp_get_wtime();
    cout << "Minimum Element (Parallel): " << min_ele << endl;
    cout << "Time Taken: " << (end - start) << " seconds" << endl;
}

void maximum(vector<int>& array) {
    int max_val = INT_MIN;
    double start = omp_get_wtime();
    for (int i = 0; i < array.size(); i++) {
        if (array[i] > max_val) max_val = array[i];
    }
    double end = omp_get_wtime();
    cout << "Maximum Element: " << max_val << endl;
    cout << "Time Taken: " << (end - start) << " seconds" << endl;

    int max_ele = INT_MIN;
    start = omp_get_wtime();
    #pragma omp parallel for reduction(max:max_ele)
    for (int i = 0; i < array.size(); i++) {
        if (array[i] > max_ele) max_ele = array[i];
    }
    end = omp_get_wtime();
    cout << "Maximum Element (Parallel): " << max_ele << endl;
    cout << "Time Taken: " << (end - start) << " seconds" << endl;
}

void sum(vector<int>& array) {
    int sum = 0;
    double start = omp_get_wtime();
    for (int i = 0; i < array.size(); i++) {
        sum += array[i];
    }
    double end = omp_get_wtime();
    cout << "Sum: " << sum << endl;
    cout << "Time Taken: " << (end - start) << " seconds" << endl;

    sum = 0;
    start = omp_get_wtime();
    #pragma omp parallel for reduction(+:sum)
    for (int i = 0; i < array.size(); i++) {
        sum += array[i];
    }
    end = omp_get_wtime();
    cout << "Sum (Parallel): " << sum << endl;
    cout << "Time Taken: " << (end - start) << " seconds" << endl;
}

void average(vector<int>& array) {
    double avg = 0.0;
    double start = omp_get_wtime();
    for (int i = 0; i < array.size(); i++) {
        avg += array[i];
    }
    double end = omp_get_wtime();
    cout << "Average: " << avg / array.size() << endl;
    cout << "Time Taken: " << (end - start) << " seconds" << endl;

    avg = 0.0;
    start = omp_get_wtime();
    #pragma omp parallel for reduction(+:avg)
    for (int i = 0; i < array.size(); i++) {
        avg += array[i];
    }
    end = omp_get_wtime();
    cout << "Average (Parallel): " << avg / array.size() << endl;
    cout << "Time Taken: " << (end - start) << " seconds" << endl;
}

int main() {
    int N;
    const int MAX = 1000;
    cout << "Enter number of elements: ";
    cin >> N;

    srand(time(0));
    vector<int> array(N);
    for (int i = 0; i < N; i++) {
        array[i] = rand() % MAX;
    }

    minimum(array);
    maximum(array);
    sum(array);
    average(array);

    return 0;
}
