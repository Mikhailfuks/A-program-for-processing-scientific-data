#include <iostream>
#include <fstream>
#include <vector>
#include <string>
#include <sstream>
#include <algorithm>
#include <cmath>
#include <iomanip>

using namespace std;

// Структура для хранения данных
struct DataPoint {
    double x;
    double y;
};

// Функция для загрузки данных из файла
vector<DataPoint> loadData(const string& filename) {
    vector<DataPoint> data;
    ifstream file(filename);
    string line;

    while (getline(file, line)) {
        stringstream ss(line);
        double x, y;
        ss >> x >> y;
        data.push_back({x, y});
    }

    file.close();
    return data;
}

// Функция для вычисления среднего значения
double calculateMean(const vector<DataPoint>& data) {
    double sum = 0;
    for (const DataPoint& point : data) {
        sum += point.y;
    }
    return sum / data.size();
}

// Функция для вычисления стандартного отклонения
double calculateStdDev(const vector<DataPoint>& data, double mean) {
    double sumSquaredDiff = 0;
    for (const DataPoint& point : data) {
        sumSquaredDiff += pow(point.y - mean, 2);
    }
    return sqrt(sumSquaredDiff / data.size());
}

// Функция для линейной регрессии
pair<double, double> linearRegression(const vector<DataPoint>& data) {
    double sumX = 0, sumY = 0, sumXY = 0, sumXX = 0;
    for (const DataPoint& point : data) {
        sumX += point.x;
        sumY += point.y;
        sumXY += point.x * point.y;
        sumXX += point.x * point.x;
    }

    int n = data.size();
    double slope = (n * sumXY - sumX * sumY) / (n * sumXX - sumX * sumX);
    double intercept = (sumY - slope * sumX) / n;

    return {slope, intercept};
}

// Функция для вывода данных
void printData(const vector<DataPoint>& data) {
    cout << "Данные:" << endl;
    cout << "x" << "\t" << "y" << endl;
    for (const DataPoint& point : data) {
        cout << point.x << "\t" << point.y << endl;
    }
}

// Функция для вывода результатов анализа
void printResults(const vector<DataPoint>& data) {
    double mean = calculateMean(data);
    double stdDev = calculateStdDev(data, mean);
    pair<double, double> regression = linearRegression(data);

    cout << "\nРезультаты анализа:" << endl;
    cout << "Среднее значение: " << mean << endl;
    cout << "Стандартное отклонение: " << stdDev << endl;
    cout << "Линейная регрессия: y = " << regression.first << "x + " << regression.second << endl;
}

int main() {
    string filename = "data.txt"; // Имя файла с данными

    // Загружаем данные из файла
    vector<DataPoint> data = loadData(filename);

    // Выводим данные
    printData(data);

    // Выводим результаты анализа
    printResults(data);

    return 0;
}
