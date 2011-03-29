require 'yaml'
require 'ap'
require 'colorize'

VALID_FACTIONS      = ['Jandar']
VALID_SETS          = ['Rise of the Valkyrie']
VALID_SPECIES       = ['Kyrie', 'Human']
VALID_PERSONALITIES = ['Merciful', 'Valiant', 'Disciplined']
VALID_CLASSES       = ['Warrior', 'Soldier', 'Soldiers']
VALID_ORIGINS       = ['Valhalla', 'Earth']


def unit_validation_errors(unit)
  errors = Array.new

  errors << "Unit must have a name.".colorize(:red) if unit['name'].to_s.empty?
  errors << unit['name'].colorize(:yellow) + ' must have a valid faction.'.colorize(:red)                               unless VALID_FACTIONS.include?              unit['faction']
  errors << unit['name'].colorize(:yellow) + ' must have a valid set.'.colorize(:red)                                   unless VALID_SETS.include?                  unit['set']
  errors << unit['name'].colorize(:yellow) + ' must have a valid species'.colorize(:red)                                unless VALID_SPECIES.include?               unit['species']
  errors << unit['name'].colorize(:yellow) + ' must have a valid origin'.colorize(:red)                                 unless VALID_ORIGINS.include?               unit['origin']
  errors << unit['name'].colorize(:yellow) + ' must have a hero value of "Yes" or "No".'.colorize(:red)                 unless %w(Yes No).include?                  unit['hero']
  errors << unit['name'].colorize(:yellow) + ' must have a rarity of "Unique", "Uncommon" or "Common".'.colorize(:red)  unless %w(Unique Uncommon Common).include?  unit['rarity']
  errors << unit['name'].colorize(:yellow) + ' must have a valid personality.'.colorize(:red)                           unless VALID_PERSONALITIES.include?         unit['personality']
  errors << unit['name'].colorize(:yellow) + ' must have a valid class.'.colorize(:red)                                 unless VALID_CLASSES.include?               unit['class']
  errors << unit['name'].colorize(:yellow) + ' must have a size of "Medium".'.colorize(:red)                            unless %w(Medium).include?                  unit['size']
  errors << unit['name'].colorize(:yellow) + ' must have a numerical height above 0.'.colorize(:red)                    unless unit['height'].is_a?(Fixnum) and unit['height'] > 0
  errors << unit['name'].colorize(:yellow) + ' must have a spaces_per_figure of 1 or 2.'.colorize(:red)                 unless [1,2].include?                       unit['spaces_per_figure']
  errors << unit['name'].colorize(:yellow) + ' must have a collector_number like NN/NN.'.colorize(:red)                 unless unit['collector_number'] =~ /\d+\/\d+/
  errors << unit['name'].colorize(:yellow) + ' must have a biography.'.colorize(:red)                                   if unit['biography'].to_s.empty?

  errors << unit['name'].colorize(:yellow) + " must have a numerical basic move above 0".colorize(:red)                 unless unit['stats']['basic']['move'].to_i > 0
  errors << unit['name'].colorize(:yellow) + " must have a numerical basic range above 0".colorize(:red)                unless unit['stats']['basic']['range'].to_i > 0
  errors << unit['name'].colorize(:yellow) + " must have a numerical basic attack above 0".colorize(:red)               unless unit['stats']['basic']['attack'].to_i > 0
  errors << unit['name'].colorize(:yellow) + " must have a numerical basic defense above 0".colorize(:red)              unless unit['stats']['basic']['defense'].to_i > 0

  errors << unit['name'].colorize(:yellow) + " must have a numerical advanced life above 0".colorize(:red)              unless unit['stats']['advanced']['life'].to_i > 0
  errors << unit['name'].colorize(:yellow) + " must have a numerical advanced move above 0".colorize(:red)              unless unit['stats']['advanced']['move'].to_i > 0
  errors << unit['name'].colorize(:yellow) + " must have a numerical advanced range above 0".colorize(:red)             unless unit['stats']['advanced']['range'].to_i > 0
  errors << unit['name'].colorize(:yellow) + " must have a numerical advanced attack above 0".colorize(:red)            unless unit['stats']['advanced']['attack'].to_i > 0
  errors << unit['name'].colorize(:yellow) + " must have a numerical advanced defense above 0".colorize(:red)           unless unit['stats']['advanced']['defense'].to_i > 0
  errors << unit['name'].colorize(:yellow) + " must have a numerical advanced points above 0".colorize(:red)            unless unit['stats']['advanced']['points'].to_i > 0

  errors << unit['name'].colorize(:yellow) + " must have at least one advanced power".colorize(:red)                    unless unit['stats']['advanced']['powers'].to_a.count > 0

  unit['stats']['advanced']['powers'].to_a.each do |power|
    errors << unit['name'].colorize(:yellow) + "'s powers must have a name".colorize(:red)        if power['name'].to_s.empty?
    errors << unit['name'].colorize(:yellow) + "'s powers must have a description".colorize(:red) if power['description'].to_s.empty?
  end

  if unit['hero'] == "No"
    errors << unit['name'].colorize(:yellow) + ' needs a squad size above 0.'.colorize(:red) unless unit['squad_size'].to_i > 0
  end

  errors << unit['name'].colorize(:yellow) + ' needs a basic card scan.'.colorize(:red) unless File.exists? "assets/units/#{unit['name'].downcase.gsub(' ', '_')}/basic.jpeg"
  errors << unit['name'].colorize(:yellow) + ' needs an advanced card scan.'.colorize(:red) unless File.exists? "assets/units/#{unit['name'].downcase.gsub(' ', '_')}/advanced.jpeg"
  errors << unit['name'].colorize(:yellow) + ' needs a mini picture.'.colorize(:red) unless File.exists? "assets/units/#{unit['name'].downcase.gsub(' ', '_')}/mini.jpeg"

  return errors
end

task :default do
  units = YAML::load( File.open('units.yaml' ) )
  invalid_units = Array.new

  units.each do |unit|
    if unit_validation_errors(unit).empty?
      print ".".colorize(:green)
    else
      print ".".colorize(:red)
      invalid_units << unit
    end
  end
  puts
    invalid_units.each do |invalid_unit|
    puts unit_validation_errors(invalid_unit).join("\n")
  end
  puts "=-=-=-=-=-=-=-=-=-=-=-=".colorize(:light_blue)
  puts "Total Units:\t#{units.count}".colorize(:light_blue)
end
