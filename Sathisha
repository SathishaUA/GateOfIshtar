#include <iostream>
#include <string>
#include <ctime>
#include <vector>
#include <algorithm>
#include <ctype.h>
#include <map>

#define INVINCIBLE_CHAMPION  {std::string("WIZARD"), std::string("SPIRIT") }

enum class day
{
    SUNDAY = 0,
    MONDAY,
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY
};

class RollGame
{
    public:
        RollGame() = default;
        ~RollGame() = default;
        uint8_t calculate_champion_health(std::string, std::vector<std::string>);
        uint8_t calculate_damage_taken(const struct tm&, std::string);
        bool holly_day(const struct tm&);
        bool invincible_champion(std::string);
    private:
        uint8_t humarn_hp  = 100;
        uint8_t wizard_hp  = 100;
        uint8_t spirit_hp  = 100;
        uint8_t giant_h    = 150;
        uint8_t vampire_hp = 110;

        std::map<std::string, uint8_t> champHealth = {
            { "human",   humarn_hp   },
            { "wizard",  wizard_hp   },
            { "spirit",  spirit_hp   },
            { "giant",   giant_h     },
            { "vampire", vampire_hp  }
        };
};

bool RollGame::holly_day(const struct tm& date)
{
    return (date.tm_wday == static_cast<int>(day::TUESDAY) or
            date.tm_wday == static_cast<int>(day::THURSDAY)) ? true : false;
}

bool RollGame::invincible_champion(std::string champion)
{
    std::transform(champion.begin(), champion.end(), champion.begin(), ::toupper);
    bool retValue = false;
    std::find_if(std::begin(INVINCIBLE_CHAMPION), std::end(INVINCIBLE_CHAMPION),
            [&](const std::string& item)
            {
            retValue = (item == champion)? true : false;
            return retValue;
            });
    return retValue;

}

uint8_t RollGame::calculate_damage_taken(const struct tm& date, std::string champion)
{
    uint8_t damage_taken = 0;
    if (holly_day(date) or invincible_champion(champion))
    {
        return 0;
    }
    // "Janna" demon of Wind spawned
    else if (date.tm_hour == 6 and date.tm_min >= 0 and date.tm_min <= 29)
    {
        damage_taken = 7;
    }
    // "Tiamat" goddess of Oceans spawned
    else if (date.tm_hour == 6 and date.tm_min  >= 30 and date.tm_min <= 59)
    {
        damage_taken = 18;
    }
    // "Mithra" goddess of sun spawned
    else if (date.tm_hour == 7 and date.tm_min  >= 0 and date.tm_min <= 59)
    {
        damage_taken = 25;
    }
    // "Warwick" God of war spawned
    else if (date.tm_hour == 8 and date.tm_min  >= 0 and date.tm_min <= 29)
    {
        damage_taken = 18;
    }
    // "Kalista" demon of agony spawned
    else if (date.tm_hour >= 8 and date.tm_hour <= 14 and date.tm_min  >= 30 and date.tm_min <= 59)
    {
        damage_taken = 7;
    }
    // "Ahri" goddess of wisdom spawned
    else if (date.tm_hour == 15 and date.tm_min >= 0 and date.tm_min <= 29)
    {
        damage_taken = 13;
    }
    // "Brand" god of fire spawned
    else if (date.tm_hour == 15 and date.tm_min >= 0 or date.tm_hour == 16 and date.tm_min <= 59)
    {
        damage_taken = 25;
    }
    // "Rumble" god of lightning spawned
    else if (date.tm_hour == 17 and date.tm_min >= 0 and date.tm_min <= 59)
    {
        damage_taken = 18;
    }
    // "Skarner" the scorpion demon spawned
    else if (date.tm_hour >= 18 and date.tm_hour <= 19 and date.tm_min >= 0 and date.tm_min <= 59)
    {
        damage_taken = 7;
    }
    // "Luna" The goddess of the moon spawned
    else if (date.tm_hour == 20 and date.tm_min <=59)
    {
        damage_taken = 13;
    }
    else
    {
        damage_taken = 0;
    }

    return damage_taken;
}

uint8_t RollGame::calculate_champion_health(std::string champion,
        std::vector<std::string> date_string_intervals_vec)
{
    uint8_t total_damage = 0;

    for (std::size_t i = 0; i < date_string_intervals_vec.size(); i++)
    {
        struct tm date, date_next;
        if (strptime(date_string_intervals_vec.at(i).c_str(), "%Y-%m-%d %H:%M", &date))
        {
            if (i+1 < date_string_intervals_vec.size())
            {
                if (strptime(date_string_intervals_vec.at(i+1).c_str(), "%Y-%m-%d %H:%M", &date_next))
                {
                    std::cout << "date_next is:" << date_string_intervals_vec.at(i+1) << std::endl;
                }
                else
                {
                    continue;
                }
            }
            else
            {
                date_next = date;
            }
        }
        else
        {
            continue;
        }

        auto interval = difftime(mktime(&date_next), mktime(&date));
        auto  next_damage = calculate_damage_taken(date, champion);

        if (interval >= 3600 or ((i+1) == date_string_intervals_vec.size()))
        {
            total_damage += next_damage;
        }

    }
    std::cout<<"Champion's initial HP: "<< (int) champHealth[champion]
        <<"\t total_damage :"<<(int) total_damage<<std::endl;
    return (champHealth[champion] - total_damage);
}

int main()
{
    RollGame rollGame;
    std::string champion = "human";
    std::vector<std::string> date_string_intervals_vec
    {"2020-03-2 6:05","2020-03-2 6:10", "2020-03-2 6:15", "2020-03-2 6:20"};

    int remainingHP = rollGame.calculate_champion_health(champion, date_string_intervals_vec);

    std::cout <<"Remaining HP of champion :"<<remainingHP<<std::endl;

    return 0;
}
