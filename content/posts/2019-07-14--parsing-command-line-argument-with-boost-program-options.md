---
title: Parsing command line argument with boost program options
date: "2019-07-14T10:00:00Z"
template: "post"
draft: false
slug: "/posts/parsing-command-line-argument-with-boost-program-options/"
category: "programming"
tags:
  - "c++"
description: "Boilerplate to parse command line arguments in c++ with boost::program_options."
socialImage: ""
---

1. Copy and past this code
2. modify sequences after "desc.add_options()"
3. use object "input" as a container of command line arguments

```cpp
#pragma once
#include <exception>
#include <iostream>
#include <string>
#include <boost/program_options.hpp>


struct Input
{
	std::string input1_without_option;
	std::string input2_without_option;
	bool input_with_option;
	bool is_to_show_help;

	Input()
		: input_with_option(false)
		, is_to_show_help(false) {}
};


inline bool ParseOptions(int argc, char* argv[], Input& input)
{
	using namespace boost::program_options;

	variables_map vm;
	try
	{
		// define options
		options_description desc("options");
		desc.add_options()
			("my_option,o", "sample option")
			("help,h", "help")
			("unnamed", value<std::vector<std::string>>(), "unnamed")	// this name must be same as one which is input into positional_options_description
			;

		positional_options_description positional_desc;
		positional_desc.add("unnamed", 2);	// this is for inputs without option. the number of arguements without options must be 2

		// get arguments
		store(
			command_line_parser(argc, argv)
			.options(desc)
			.positional(positional_desc)
			.run()
		, vm);
		notify(vm);
	}
	catch(...)
	{
		std::cout << "error: invalid argument\n";
		return false;
	}
	
	input.is_to_show_help = vm.count("help") ? true : false;
	if(!input.is_to_show_help)
	{
		const auto& unnameds = vm["unnamed"].as<std::vector<std::string>>();
		if(2 > unnameds.size())
		{
			std::cout << "error: invalid argument\n";
			return false;
		}
		input.input1_without_option = unnameds[0];
		input.input2_without_option = unnameds[1];
		input.input_with_option = vm.count("my_option") ? true : false;
	}
	return true;
}
```

