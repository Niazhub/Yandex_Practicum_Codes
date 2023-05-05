#include <cassert>
#include <iostream>
#include <map>
#include <string>
#include <vector>
#include <sstream>

using namespace std;

enum class QueryType {
    NewBus,
    BusesForStop,
    StopsForBus,
    AllBuses,
};

struct Query {
    QueryType type;
    string bus;
    string stop;
    vector<string> stops;
};

istream& operator>>(istream& is, Query& q) {
    string query;
    is >> query;
    if (query == "NEW_BUS"s) {
        q.type = QueryType::NewBus;
        is >> q.bus;
        int stop_count;
        is >> stop_count;
        q.stops.resize(stop_count);
        for (string& stop : q.stops) {
            is >> stop;
        }
        return is;
    }
    else if (query == "BUSES_FOR_STOP"s) {
        q.type = QueryType::BusesForStop;
        is >> q.stop;
        return is;
 
    }
    else if (query == "STOPS_FOR_BUS"s) {
        q.type = QueryType::StopsForBus;
        is >> q.bus;
 
        return is;
    }
    else if (query == "ALL_BUSES"s) {
        q.type = QueryType::AllBuses;
        return is;
    }
    return is;
}

struct BusesForStopResponse {
    map<string, vector<string>> buses_for_stop;
};

ostream& operator<<(ostream& os, const BusesForStopResponse& r) {
    if(r.buses_for_stop.empty()){
        os << "No stop";
        return os;
    }
    for (auto& [stop, buses] : r.buses_for_stop) {
        int i = 0;
        for (const string& bus : buses) {
            i++;
            os << bus;
            if(i != bus.size()){
                os << " "s;
            }
        }
    }
    return os;
}

struct StopsForBusResponse {
    vector<pair<string, vector<string>>> stop_for_buses;
};

ostream& operator<<(ostream& os, const StopsForBusResponse& r) {
    if (r.stop_for_buses.empty()) {
        os << "No bus"s;
        return os;
    }
    int i = 0;
    for (auto& [stop, buses] : r.stop_for_buses) {
        i++;
        os << "Stop "s << stop << ": "s;
        if (buses.size() == 1) {
            os << "no interchange"s;
        } else {
            int j = 0;
            for (const string& other_bus : buses) {
                j++;
                if(other_bus != ""){
                    os << other_bus;
                }
                if(j != buses.size() - 1){
                    os << " "s;
                }
            }
        }
        if(i != r.stop_for_buses.size()){
            os << endl;
        }
    }
    return os;
}

struct AllBusesResponse {
    map<string, vector<string>> all_buses;
};

ostream& operator<<(ostream& os, const AllBusesResponse& r) {
    if(r.all_buses.empty()){
        os << "No buses"s;
        return os;
    } else{
        int i = 0;
        for(const auto& b : r.all_buses){
            i++;
            os << "Bus "s << b.first << ": "s;
            int j = 0;
            for(const auto& s : b.second){
                j++;
                os << s;
                if(j != b.second.size()){
                    os << " "s;
                }
            }
            if(i != r.all_buses.size()){
                os << endl;
            }
        }
    }
    return os;
}

class BusManager {
public:
    AllBusesResponse bus_1;
    StopsForBusResponse bus_2;
    BusesForStopResponse bus_3;

    void AddBus(const string& bus, const vector<string>& stops) {
        vector<string> bus_;
        for (const string& stop : stops) {
            bus_1.all_buses[bus].push_back(stop);
            bus_.push_back(stop);
            bus_3.buses_for_stop[stop].push_back(bus);
        }
        bus_2.stop_for_buses.push_back({bus, bus_});
    }

    BusesForStopResponse GetBusesForStop(const string& stop) const {
        BusesForStopResponse r;
        if (bus_3.buses_for_stop.count(stop) != 0) {
            for (const string& bus : bus_3.buses_for_stop.at(stop)) {
                r.buses_for_stop[stop].push_back(bus);
            }
        }
        return r;
    }

    StopsForBusResponse GetStopsForBus(const string& bus) const {
        StopsForBusResponse r;
        
        if(!bus_2.stop_for_buses.empty()){
            for(int i = 0; i < bus_2.stop_for_buses.size(); i++) {
                if(bus_2.stop_for_buses[i].first == bus){
                    for (const string& stop : bus_2.stop_for_buses[i].second) {
                        vector<string> s1;
                        for (const string& other_bus : bus_3.buses_for_stop.at(stop)) {
                            if(bus != other_bus){
                                s1.push_back(other_bus);
                            } else{
                                s1.push_back("");
                            }
                        }
                        r.stop_for_buses.push_back({stop, s1});
                    }
                }
                
            }
        }
        
        return r;
    }

    AllBusesResponse GetAllBuses() const {
        AllBusesResponse r;
        if (!bus_1.all_buses.empty()) {
            for (const auto& bus_item : bus_1.all_buses) {
                for (const string& stop : bus_item.second) {
                    r.all_buses[bus_item.first].push_back(stop);
                }
            }
        } 
        return r;
    }
};

int main() {
    int query_count;
    Query q;
    
    cin >> query_count;

    BusManager bm;
    for (int i = 0; i < query_count; ++i) {
        cin >> q;
        switch (q.type) {
            case QueryType::NewBus:
                bm.AddBus(q.bus, q.stops);
                break;
            case QueryType::BusesForStop:
                cout << bm.GetBusesForStop(q.stop) << endl;
                break;
            case QueryType::StopsForBus:
                cout << bm.GetStopsForBus(q.bus) << endl;
                break;
            case QueryType::AllBuses:
                cout << bm.GetAllBuses() << endl;
                break;
        }
    }
}
